# Content Protection
_informational reference for the DRM formats and the HDCP versions supported on the Roku Platform_

**Sections:**

* [DRM](#drm)
 * [Adobe Access](#adobe-access)
 * [PlayReady](#playready)
 * [Verimatrix](#verimatrix)
* [Copy Protection](#copy-protection)

---

## DRM

Although AES-128 Encryption is also supported for HLS, Roku recommends using Adobe DRM because there is no DRM when using AES-128 Encryption.

|        | PlayReady | Adobe DRM | Verimatrix | AES-128 |
| ------ | :-------: | :-------: | :--------: | :-----: |
| HLS    || :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Smooth | :heavy_check_mark: |||                    |
| DASH   | :heavy_check_mark: |||                    ||

> Supported DRM info can be queried using [ifDeviceInfo.getDRMInfo()](https://sdkdocs.roku.com/display/sdkdoc/ifDeviceInfo).

### Adobe Access

**Required Roku channel manifest entries:**

```
requires_aaxs_drm=1
requires_aaxs_version=1.0
```

**Configure DRM parameters in an roAssociativeArray:**

```brightscript
drmParams = createObject("roAssociativeArray")
drmParams.name = "AdobeAccess"
drmParams.appData = "drm-token-as-a-string" 'the DRM token (if required) that needs to be passed to the license server
```

**Setup content metadata and set to video node:**

```brightscript
contentNode = createObject("roSGNode", "contentNode")
contentNode.streamFormat = "hls"
contentNode.url = "wwww.myvideo.com/content.m3u8"
contentNode.drmParams = drmParams

m.video.content = contentNode
```

### PlayReady

> No Roku channel manifest entries are required for PlayReady.

**Configure DRM parameters in an roAssociativeArray:**

```brightscript
drmParams = createObject("roAssociativeArray")
drmParams.encodingType = "PlayReadyLicenseAcquisitionUrl"
drmParams.encodingKey = "PlayReadyLicenseServerUrl"
```

If your PlayReady implementation requires custom request data, `encodingType` and `encodingKey` should be formatted like the following:

```brightscript
drmParams = createObject("roAssociativeArray")
drmParams.encodingType = "PlayReadyLicenseAcquisitionAndChallenge"
drmParams.encodingKey = "PlayReadyLicenseServerUrl" + "%%%" + customData
```

**Setup content metadata and set to video node:**

```brightscript
contentNode = createObject("roSGNode", "contentNode")
contentNode.streamFormat = "smooth"
contentNode.url = "wwww.myvideo.com/content.ism"
contentNode.drmParams = drmParams

m.video.content = contentNode
```

### Verimatrix

**Required Roku channel manifest entries:**

```
requires_verimatrix_drm=1
requires_verimatrix_version=1.0
```

**Configure DRM parameters in an roAssociativeArray:**

```brightscript
drmParams = createObject("roAssociativeArray")
drmParams.name = "Verimatrix"
drmParams.authDomain = "auth-value-from-streaming-provider"
drmParams.serializationUrl = "hostname-url-from-streaming-provider"
```

**Setup content metadata and set to video node:**

```brightscript
contentNode = createObject("roSGNode", "contentNode")
contentNode.streamFormat = "hls"
contentNode.url = "wwww.myvideo.com/content.m3u8"
contentNode.drmParams = drmParams

m.video.content = contentNode
```

## Copy Protection

Roku's OS also supports HDCP for content copy protection between the Roku player's HDMI port and the connected display. However, the HDCP version depends on the Roku Model and the Display Type that it's currently set to.

|      | Roku 4K capable devices | All other Roku devices |
| ---- | :---------------------: | :--------------------: |
| TEE  | Yes | No
| HDCP | 2.2 <sup>1</sup> | 1.4

> <sup>1</sup> 4K devices set to a Display Type with a resolution smaller than 4K will default to HDCP 1.4.

> HDCP versioning can be queried using [ifHdmiStatus.getHDCPVersion()](https://sdkdocs.roku.com/display/sdkdoc/ifHdmiStatus).
