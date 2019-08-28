### Instantiate client object
```python
import qopy
client = qopy.Client()
```
### Authorization ###
```python
client.auth('email_address', 'password', 'app_id')
```
Password must be MD5 hashed.   
If you plan to use methods which don't require an account, you may use the setAppId method instead.
```python
client.set_app_id('app_id')
```
- If you attempt to use an auth-required method without first authorizing, the `unauthenticatedError` exception will be thrown.
- If you attempt to use a non-auth-required method, but haven't set an app Id, the same exception will be thrown.
### Demos ###
#### Fetching single track metadata ####
```python
meta = client.get_track_metadata(60896649)
print(meta.track_num, meta.album_title)
```
Output:
`4 Untrue`
#### Fetching album metadata ####
```python
meta = client.get_album_metadata('q9vy3z7yknvgc')
print(meta.track_count, meta.album_title)
```
Output: 
```
13 Untrue
```
Iteration:
```python
for title, track_num in zip(meta.track_title, meta.track_num):
	print(track_num, title)
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
track_url = client.get_fle(60896649, 27).url
print(track_url)
```
Output: 
```
https://streaming2.qobuz.com/file?uid=x&eid=x&fmt=6&profile=raw&app_id=x&x&etsp=xhmac=x
```
Every track in an album:
```python
for track_id in client.get_album_metadata('q9vy3z7yknvgc').track_id:
	print(client.get_file(track_id, 27).url)
```
Output: 
```
https://streaming2.qobuz.com/file?uid=x&eid=x&fmt=6&profile=raw&app_id=x&x&etsp=xhmac=x
x13
```
### Methods ###
#### auth() ####
```python
client.auth('email_address', 'password', 'app_id')
```
Password must be MD5 hashed.
#### get_album_metadata() ####
```python
client.get_album_metadata('album ID')
```
#### get_file() ####
```python
client.get_file('track_id', 'format_id')
```

#### get_track_metadata() ####
```python
client.get_track_metadata('track_id')
```
Returned keys:

#### is_auth() ####
```python
client.is_auth()
```
Check if user is authenticated. 
`True` will be returned if so, and `False` if not.

#### set_app_id() ####
