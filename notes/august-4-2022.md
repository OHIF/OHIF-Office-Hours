- NIFTI loader
  - Development of the nifti loader for cornerstone3D is done internally (it is based on https://github.com/rii-mango/NIFTI-Reader-JS), and it will hopefully be available to public in the next few months
  - legacy loader, request nifti gzip decoded, and if you give it parameters it will give back the slice itself without any web workers
  - The idea of using webworkers for
- sigmoid VOI LUTs
  - it should be done vtk.js with actor transfer function
  - set/apply the sigmoid function
  - legacy cornerstone has non linear look up table ([https://github.com/cornerstonejs/cornerstone/blob/master/src/internal/getVOILut.js#L40](https://github.com/cornerstonejs/cornerstone/blob/master/src/internal/getVOILut.js#L40))
  - Initial implementation can be on setProperties [https://github.com/cornerstonejs/cornerstone3D-beta/blob/main/packages/core/src/RenderingEngine/StackViewport.ts#L544-L552](https://github.com/cornerstonejs/cornerstone3D-beta/blob/main/packages/core/src/RenderingEngine/StackViewport.ts#L544-L552)
- SUV is showing up for the PT even if not scaled
  - this is a bug and needs to be fixed
- Removing the MO from the annotations details
  - Per Salim’s comment we will remove the MO since the we don’t know the unit if it does not exist
- Extension and modes and how to create them
  - OHIF cli [https://v3-docs.ohif.org/development/ohif-cli/](https://v3-docs.ohif.org/development/ohif-cli/)