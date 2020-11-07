# Cleaning up dangling images from multi-stage image builds 
**Author:** [Alan Mills]
**Date:** [24 August 2017 19:22]
**Tags:** [Docker]
**Status**: Publish

When using [Docker multi-stage builds](), the intermediary stages prior to the final stage are stored as dangling images.  These dangling images are useful for debuging failing multi-stage builds however, you can quickly create a lot of these.  To clean up dangling images, run the command `docker rmi ``docker images -f "dangling=true" -q`
