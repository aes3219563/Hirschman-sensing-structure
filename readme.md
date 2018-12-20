# Hirschman-sensing-structure
The compressive sensing (CS), enables us to recover a sparse signal from its few linear measurements.<br>It is known that systems sensitive at the sampling node take the most advantage of CS.<br>Examples:<br>
　　Using CS, the MRI system reduces the irradiation time on the location of lesions<br>
　　Using CS, the large-area monitoring system increases lifetime since each scan costs less energy<br>
However, the CS requires many computations on its decoding.　On the platform of Python 3.7.0 (win-10@x64, i7-6700@3.4GHz), it takes 22.7 secs to recover a 256-point grayscaled image from its 128 times random linear measurements using a single-thread Orthogonal Matching Pursuit algorithm.<br>
We generate the Hirschman Transform Matrix (HTM) by interpolating and circular shifting of a DFT matrix. It has been proved that all the HTMs satisfy the Mutual Incoherent Property (MIP) and can be used as the sensing structures in CS.<br>
Examples:<br>
　　HTM(2x128) means we use 127 zeros to interpolate the 2-point DFT and then circularly shift it and finally get the 256-point HTM<br>  　　HTM(128x2) means we use 1 zero to interpolate the 128-point DFT and then circularly shift it and finally get the 256-point HTM<br>
For a 256-point HTM, we have HTM(2X128),HTM(4X64),HTM(8X32),HTM(16X16),HTM(32X8),HTM(64X4),HTM(128X2) and HTM(1x256) eight choices in total. Among them, HTM(2X128) is sparse, HTM(128X2) is dense, and HTM(1x256) is equal to the 256-point DFT.<br>
On the same platform, compared to the DFT sensing structure:<br>
　　HTM(128x2) takes 18.1 secs to do the recovery (speed increases by 20.3%)<br>
　　HTM(32x8) takes 15.986 secs to do the recovery (speed increases by 29.6%)<br>
　　HTM(2x128) takes 5.3 secs to do the recovery (speed increases by 76.7%)<br>
The sparse HTM is super fast, but its recovery performance heavily depends on how the target image looks like or more specifically, depends on how it looks like after sparsification.<br> 
We run this project to collect the recovery performances of different HTMs based on multiple types of input images. We would like to use statistical ways and [machine learning](https://github.com/aes3219563/SSD-Model-for-small-object-recognization) to identify how can we pick up the appropriate HTMs for different images to maximize speed of CS recovery algorithm.<br>
We offer 4 APIs to collect data, the algorithms on the server are also provided in our [Hirschman Transform Lib](https://github.com/aes3219563/Hirschman-Transform-Lib):<br>
1.Submit target image:<br>
　　http://www.pssautohelper.com:1988/HirschmanPost?r=*&c=*&s=*&k=*<br>
r: number of rows (height) of target image<br>
c: number of columns (width) of target image<br>
s: number of linear measurements<br>
k: dimension of DFT matrix used to generate the HTM<br>
target image should be attached as files in base64 format.<br>
Python example:<br>
```Python
import base64
import requests
import json
url='http://www.pssautohelper.com:1988/HirschmanPost?r=256&c=256&s=128&k=128'
with open("clock.tiff", "rb") as image_file:
    f=image_file.read()
f=base64.b64encode(f)
files={'img':f}
r=requests.post(url,files=files, stream=True)
j=json.loads(r.text)
#{'result':True,'message':'success','imgid':******}
```
2.Get recovered image:<br>
　　http://www.pssautohelper.com:1988/HirschmanGet?imgid=****<br>
imgid: server returns this id when you submit a target image<br>
response is in json format and image data is a base64 string<br>
Python example to show recovered image:<br>
```Python
import base64
import requests
import PIL
import io
import json
url='http://www.pssautohelper.com:1988/HirschmanGet?imgid=f8ea14a5_16*256*100'
r=requests.get(url)
j=json.loads(r.text)
imgdata=j['img']
imgdata=base64.b64decode(imgdata)
img=PIL.Image.open(io.BytesIO(imgdata))
img.show()
```
3.Del a imageid:<br>
　　http://www.pssautohelper.com:1988/HirschmanDel?imgid=****<br>
response is in json format<br>
Python example to delete a imgid:<br>  
```Python
import requests
import json
url='http://www.pssautohelper.com:1988/HirschmanDel?imgid=f8ea14a5_128*256*100'
r=requests.post(url)
j=json.loads(r.text)
#　{'result':True,'message':'success'}
```
For the same target image, you'll get different recovered results based on combinations of s and k. At the same measurement level (s equals), please sort those recovered images by recovery performance and leave a feedback via<br>
4.Sort the recovered images:<br>
　　http://www.pssautohelper.com:1988/HirschmanSort<br>
Python example to leave a feedback:<br>
```Python
import requests
import json
sort={'sort':['f8ea14a5_128*256*128','f8ea14a5_64*256*128','f8ea14a5_32*256*128']} # performance goes lower from left to right
r=requests.post(url,data=json.dumps(sort))
j=json.loads(r.text)
#　{'result':True,'message':'success'}
```
When a new feedback submitted, old one will be replaced. Our computational programs are running on a linux server@2.0GHz. It takes 10~20 secs to fully recover a 256-point image. We would like to add [parallel computing](https://github.com/aes3219563/Parallel-Computing) to increase response speed.

    
