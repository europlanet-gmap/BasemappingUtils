# Instructions in and modified from http://lunokhod.org/?p=468
# This workflow is ideal for large datasets and makes use of parallel computing for single-threaded tasks (such as cam2map)
# Before starting be sure to have installed at least ISIS 3.x and parallel and/or gnu-parallel from https://www.gnu.org/software/parallel/

parallel mroctx2isis from={} to={.}.cub ::: *.IMG

parallel spiceinit from={} ::: *.cub

#con il web service
#parallel spiceinit web=true from={} ::: *.cub

parallel ctxcal from={} to={.}.cal.cub ::: *.cub

parallel ctxevenodd from={} to={.}.eo.cub ::: *.cal.cub
rm *cal.cub && rm *W.cub

#Before making control networks make a cube.lis file which has the list of cubes

parallel footprintinit from={} ::: *cal.eo.cub
findimageoverlaps from=cube.lis overlaplist=overlap.lis

#To create control points
autoseed fromlist=cube.lis overlaplist=overlap.lis onet=control.net deffile=autoseed.def networkid=ctx pointid=???? description=mars

#The autoseed.def file has the following format and must be created
#Group = PolygonSeederAlgorithm
 #     Name = Grid
  #    MinimumThickness = 0.1
   #   MinimumArea = 1
    #  XSpacing = 8000
     # YSpacing = 8000
#End_Group

#To visualize the control network with few images use qnet and select the cubes

#To do this automatically use

pointreg fromlist=cube.lis cnet=control.net onet=control_pointreg.net deffile=autoRegTemplate.def

#With the file autoRegTemplate.def with following format:

#Object = AutoRegistration
 #  Group = Algorithm
  #   Name         = MaximumCorrelation
   #  Tolerance    = 0.7
#   EndGroup

 #  Group = PatternChip
  #   Samples = 19
   #  Lines   = 19
    # MinimumZScore = 1.5
     #ValidPercent = 80
   #EndGroup

   #Group = SearchChip
    # Samples = 75
     #Lines   = 75
   #EndGroup
 #EndObject

jigsaw fromlist=cube.lis update=yes twist=no radius=yes cnet=control_pointreg.net onet=control_ba.net


parallel cam2map map=equirectangular.map pixres=mpp resolution=0.5 from={} to={/.}.map.cub ::: *.echo.cub


automos from=cubelist.lis to=mosaic.cub

#Sometimes is better to crop mosaics and generate tiled tiffs for preserving dynamic range

gdal_translate -of GTiff automos_crop.cub automos_crop.tif

#This creates the tiled tiff
gdal_translate -of GTiff -co "TILED=YES" automos_crop.tif automos_tiled.tif

#This creates the tfw file

gdal_translate -co "TFW=YES" automos_tiled.tif automos_tiled.tif

#This populates the tiff with tiles

gdaladdo -ro -r nearest automos_tiled.tif 2 4 8 16 32 64 128 256

