#############################################################################
#
# ViSP, open source Visual Servoing Platform software.
# Copyright (C) 2005 - 2019 by Inria. All rights reserved.
#
# This software is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or
# (at your option) any later version.
# See the file LICENSE.txt at the root directory of this source
# distribution for additional information about the GNU GPL.
#
# For using ViSP with software that can not be combined with the GNU
# GPL, please contact Inria about acquiring a ViSP Professional
# Edition License.
#
# See http://visp.inria.fr for more information.
#
# This software was developed at:
# Inria Rennes - Bretagne Atlantique
# Campus Universitaire de Beaulieu
# 35042 Rennes Cedex
# France
#
# If you have questions regarding the use of this file, please contact
# Inria at visp@inria.fr
#
# This file is provided AS IS with NO WARRANTY OF ANY KIND, INCLUDING THE
# WARRANTY OF DESIGN, MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE.
#
# Description:
# ViSP configuration file.
#
# Authors:
# Fabien Spindler
#
#############################################################################

if(WITH_PUGIXML)
  # pugixml is private
  include_directories(${PUGIXML_INCLUDE_DIRS})
endif()

vp_add_module(vision visp_core OPTIONAL visp_visual_features visp_io PRIVATE_OPTIONAL ${PUGIXML_LIBRARIES})
vp_glob_module_sources()
vp_module_include_directories()
vp_create_module()

if (CMAKE_VERSION VERSION_LESS 2.8.12)
  vp_add_tests(
    SOURCES_EXCLUDE
      key-point/testKeyPoint-2.cpp
      key-point/testKeyPoint-4.cpp
    DEPENDS_ON visp_mbt visp_gui visp_io)
else()
  vp_add_tests(DEPENDS_ON visp_mbt visp_gui visp_io)
  if(USE_OGRE)
    # Add specific build flag to turn off warnings coming from libogre and libois 3rd party
    vp_set_source_file_compile_flag(test/key-point/testKeyPoint-2.cpp -Wno-unused-parameter -Wno-unused-but-set-parameter -Wno-overloaded-virtual)
    vp_set_source_file_compile_flag(test/key-point/testKeyPoint-4.cpp -Wno-unused-parameter -Wno-unused-but-set-parameter -Wno-overloaded-virtual)
 endif()
endif()

vp_set_source_file_compile_flag(src/pose-estimation/vpPoseFeatures.cpp -Wno-strict-overflow)
vp_set_source_file_compile_flag(src/pose-estimation/vpPoseRansac.cpp -Wno-strict-overflow)
vp_set_source_file_compile_flag(src/pose-estimation/vpLevenbergMarquartd.cpp -Wno-strict-overflow)
vp_set_source_file_compile_flag(src/key-point/vpKeyPoint.cpp -Wno-strict-overflow)

add_test(testKeyPoint-2-multithreaded testKeyPoint-2 -c ${OPTION_TO_DESACTIVE_DISPLAY} -p)
