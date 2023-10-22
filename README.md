# dvmkv2mp4 - Convert any Dolby Vision/HDR10+ MKV to MP4 that runs on many devices

This fork works with newest Gpac / MP4Box 2.2.x.
- Removed PGS2SRT functionality
- All sound tracks are copied as they are. Added newer Dolby Atmos logic and TrueHD copying with Dolby Atmos data. While you still need external device to have TrueHD you can let LG OLED to convert it to DD+ with spatial data, presumably (still testing).
- Same goes for DTS and DTS-HD 

## Build Docker image
Currently the image works only on x64 Linux. Also note that the final image size will be close to 6 GB which is normal as it includes the Tesseract OCR models.

To build your Docker image run the following command:
```
docker build -t dvmkv2mp4  https://github.com/tinof/dvmkv2mp4.git#main -f docker/Dockerfile
```
## Use the Docker image
The usage is similar to the normal variant but here you need to attach your folder where you would like to convert your files. You can also add the flags at the end of the command.

Example:
```
# docker run -it -u $(id -u):$(id -g) --rm -v <YOUR_FOLDER_PATH>:/convert dvmkv2mp4

docker run -it -u $(id -u):$(id -g) --rm -v /media/mkvvideos:/convert dvmkv2mp4 -l und,pol,eng -r -a
```
This example does the same which is mentioned in setion Usage.


PRs are welcome :)
