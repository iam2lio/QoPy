### Instantiate client object
```python
import qopy
client = qopy.Client()
```
### Authorization ###
```python
client.authorize('emailAddress', 'password', 'appID')
```
Password must be MD5 hashed. 
If you plan to use methods which don't require an account, you may use the setAppId method instead.
```python
client.setAppId('appID')
```
- If you attempt to use an auth-required method without first authorizing, the `unauthenticatedError` exception will be thrown.
- If you attemp to use a non-auth-required method, but haven't set an app Id, the same exception will be thrown.
### Demos ###
#### Fetching single track metadata ####
```python
meta = client.getTrackMetadata(60896649)
print(meta.tracknum, meta.albumtitle)
```
Output:
`4 Untrue`
#### Fetching album metadata ####
```python
meta = client.getAlbumMetadata('q9vy3z7yknvgc')
print(meta.trackscount, meta.albumtitle)
```
Output: 
```
13 Untrue
```
Iteration:
```python
for title, trackNum in zip(meta.tracktitle, meta.tracknum):
	print(tracknum, title)
```
Output: 
```
Untitled 1  
Archangel 2  
Near Dark 3  
Ghost Hardware 4  
Endorphin 5  
Etched Headplate 6  
In Mcdonalds 7  
Untrue 8  
Shell of Light 9  
Dog Shelter 10  
Homeless 11  
UK 12  
Raver 13  
```
#### Fetch track URLs ####
Single track:
```python
trackUrl = client.getFile(60896649, formatId=27)
print(trackUrl.url)
```
Output: 
```
https://streaming2.qobuz.com/file?uid=x&eid=x&fmt=6&profile=raw&app_id=x&x&etsp=xhmac=x
```
Every track in an album:
```python
for trackId in client.getAlbumMetadata('q9vy3z7yknvgc').trackid:
	print(client.getFile(trackId, formatId=27).url)
```
Output: 
```
https://streaming2.qobuz.com/file?uid=x&eid=x&fmt=6&profile=raw&app_id=x&x&etsp=xhmac=x
x13
```
### Methods ###
#### auth() ####
```python
client.authorize('emailAddress', 'password', 'appID')
```
Password must be MD5 hashed.
#### getAlbumMetadata() ####
```python
client.getAlbumMetadata('album ID')
```
#### getFile() ####
```python
client.getFile('trackId', formatId='formatId')
```
See Qobuz API documentation for format IDs.

Returned keys:
- trackid
- duration
- url
- formatid
- mimetype
- restrictions
- samplingrate
- bitdepth

#### getTrackMetadata() ####
```python
client.getTrackMetadata('trackId')
```
Returned keys:

#### isAuth() ####
```python
client.isAuth()
```
Check if user is authenticated. 
`True` will be returned if so, and `False` if not.

#### setAppId() ####
