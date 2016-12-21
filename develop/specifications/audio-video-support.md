# Audio and Video Support
_supported audio and video formats on Roku devices_

### Overview
It is important to understand the available encoding methods and supported formats when streaming content on Roku devices. The following information will give you a good basis and help you choose the best options for distributing content based on quality and availability.

Roku **officially** supports the following media formats:

* Video file types: `MP4`, `MOV`, `M4V`, `MKV`, `WebM`
* Video codecs: `H.264/AVC`, `HEVC/H.265`, `VP9`
* Audio file types: `AAC`, `MP3`, `WMA`, `WAV (PCM)`, `AIFF`, `FLAC`, `ALAC`, `AC3`, `EAC3`
* Streaming protocols: `HLS`, `Smooth`, `DASH`

These have been tested and/or are currently in-use. Other formats or encodings may be supported, but should be evaluated on a case by case basis.

**Sections:**

* [Streaming Protocols](#streaming-protocols)
* [Supported Video Formats](#supported-video-formats)
* [Supported Audio Formats](#supported-audio-formats)
* [Encoding Guidelines](#encoding-guidelines)
 * [Dolby Audio Features and Recommendations](#dolby-audio-features-and-recommendations)
 * [4K UHD Video Streaming Requirements](#4k-uhd-video-streaming-requirements)
 * [HDR10 Video Streaming Requirements](#hdr10-video-streaming-requirements)

---

## Streaming Protocols
Because network speeds can vary over time, a crucial aspect when providing the best experience to your viewers is providing multiple video streams of varying quality. Roku devices can then automatically select the best streaming quality based on the viewer's network connection.

Roku supports the following widely-used standard formats for adaptive bit rate switching:

* HLS (HTTP Live Streaming)
* Smooth Streaming
* DASH (Dynamic Adaptive Streaming over HTTP)

> :information_source: Live streams can also be delivered using these protocols although live streaming using DASH is in beta at this time. Please report any playback issues to [developer@roku.com](mailto:developer@roku.com).

## Supported Video Formats
Roku devices support the following file types:

* MP4
* MOV
* M4V
* MKV
* WebM<sup>1</sup>

Videos can be encoded using `H.264`, `HEVC (H.265)`, or `VP9` codecs.

|                          | H.264   | HEVC (H.265)<sup>1</sup> | VP9<sup>1</sup> |
| ------------------------ | ------- | ------------------------ | --------------- |
| Aspect Ratio<sup>2</sup> | Various | Various                  | Various
| Dimension | Various up to 1920x1080 | Various up to 3840x2160 | Various up to 3840x2160
| Video File Format | `.mp4`, `.mov`, `.m4v`, `.mkv` | `.mp4`, `.mov`, `.m4v`, `.mkv` | `.webm`, `.mkv`
| Streaming Format | HLS: `.m3u8`, `.ts` <br> Smooth: `.ismc` <br> DASH: `.dash` | HLS: `.m3u8`, `.ts` <br> Smooth: `.ismc` <br> DASH: `.dash` | DASH: `.dash`
| Input Frame Rate <sup>3</sup> | 24p, 25p, 30p, 50p, 60p | 24p, 25p, 30p, 50p, 60p | 24p, 25p, 30p, 60p<sup>6</sup>
| Color Space | Rec.709 | Rec.709, Rec.2020 | Rec.709, Rec.2020
| Profile | main, high | main, main 10 | profile 0, profile 2<sup>6</sup>
| Level | 4.1, 4.2 | 4.1, 5.0, 5.1 | n/a
| Video Mode | Constrained VBR | Constrained VBR | Constrained VBR
| Average Streaming Video Bit rate | Up to 10Mbps | Up to 40Mbps | Up to 40Mbps
| Average USB Video Bit rate | 384Kbps – 10Mbps | Up to 40Mbps | Up to 40Mbps
| Peak Video Bit rate | 1.5x average | 1.5x average | 1.5x average
| Key Frame Interval <sup>4</sup> | < 10s | < 10s | < 10s
| DRM | PlayReady for Smooth/DASH <br><br> Streaming AES-128 bit encryption for HLS | PlayReady for Smooth/DASH <br><br> Streaming AES-128 bit encryption for HLS | PlayReady for DASH <br><br> Widevine Level 1

### Device Specific Features

|         | Roku 4K capable devices | All other Roku devices |
| ------- | :---------------------: | :--------------------: |
| TEE<sup>5</sup> | Yes | No
| HDCP | 2.2 | 1.4

For more device specifications, see [Roku Devices](/roku-devices.md).

> <sup>1</sup> Only supported on **Roku 4K capable** devices.

> <sup>2</sup> The dimensions vary on a title-by-title basis depending on the source material and the target aspect ratio for the encoding (such as 4:3 or 16:9).  Content should always be encoded at full width, and the height is adjusted. For example, a 1.66 aspect ratio source is encoded as a 720x432 video and displayed as letterboxed for a 4:3 display.

> <sup>3</sup> The frame rate used for encoding depends on the source material. Film content is generally 23.976 FPS, while video content is generally at 29.97.

> <sup>4</sup> All segments should start with an IDR frame and align across all bit rate variants. The recommended segment size is < 10 seconds for VOD and < 5 seconds for live content, and the segment size should be constant.

> <sup>5</sup> Trusted Execution Environment

> <sup>6</sup> Only supported on HDR10 capable Roku devices.

## Supported Audio Formats

Roku devices support the following audio file types:

* AAC: HE-AACv2, AAC-LC (CBR)
* MP3
* WMA, WAV (PCM)
* AIFF
* FLAC
* ALAC
* Dolby Audio: Dolby Digital (AC3), Dolby Digital Plus (EAC3)
* Passthrough: DTS

|                    | AAC | AC3/EAC3 | DTS |
| ------------------ | --- | -------- | --- |
| Decode/Passthrough | Decode<sup>1</sup> | Decode<sup>2</sup> | Passthrough
| Sampling Rate      | 44.1 Khz, 48 Khz | 48Khz | Passthrough
| Sample Size        | 16-bit | 16-bit | Passthrough
| Bit rate           | 32-256 Kbps | 96-768 Kbps | Passthrough
| Number of Channels | 2.0 | 2.0, 5.1, 7.1, Atmos | Passthrough

> <sup>1</sup> Multichannel AAC is not supported on all Roku models. Roku TVs, Roku 4, and Roku Ultra set-top-boxes support multichannel decode to PCM stereo.

> <sup>2</sup> Roku TVs and Roku Ultra can decode AC3 and EAC3. All other Roku devices will do passthrough to the receiving device. Roku Ultra supports the latest Dolby technologies such as Dolby Atmos and System Sound Mixing [(MS12)](http://www.dolby.com/us/en/professional/broadcast/products/dolby-ms12.html).

## Encoding Guidelines

For typical streaming video applications, we recommend a range of about 800Kbps to about 4.0Mbps. For USB playback, we recommend that you stay under 8.0 Mbps. This provides a good balance between quality and support for a wide number of users. In some cases, lower and higher bit rates have been used, but this frequently results in a bad quality, or limits the numbers of users that can view those encodings. If content contains a surround sound track, AAC 2-channel stereo should be provided as a backup audio track.

Bit rate Recommendations:

* Roku recommends encoding a low bit rate stream around 800Kbps.
* Roku recommends the user have at least a 1.5Mbps connection.
* Some customers will have low bit rate connections around 1Mbps or experience intermittent network load that causes drops below Roku recommended network speeds.

Several content meta-data properties are also available for configuring streaming performance:

* SwitchingStrategy
* AdaptiveMinStartBitrate
* AdaptiveMaxStartBitrate
* MinBandwidth
* MaxBandwidth

> :information_source: See [Content Meta-Data](https://sdkdocs.roku.com/display/sdkdoc/Content+Meta-Data) for more information.

### Dolby Audio Features and Recommendations

With the Roku Ultra, Dolby Audio is natively integrated.
Roku recommends encoding in Dolby Digital Plus instead of Dolby Digital.

**Features include:**
* Consistent audio delivery
* System sound mixing [(MS12)](http://www.dolby.com/us/en/professional/broadcast/products/dolby-ms12.html)
* Maximum compatibility with device end points

Stereo and 5.1 content will natively decode on devices while 7.1 content will passthrough.

When encoding in Dolby Digital Plus, the following bit rates are recommended:

| Channels | Bit rate |
| -------- | -------- |
| Stereo 2.0 | 96 kbps
| Multi-channel 5.1 | 192 kbps
| Multi-channel 7.1 | 384 kbps

> Developers can encode video content using services like Azure or Encoding.com.
>
> :information_source: For more information, visit http://developer.dolby.com

### 4K UHD Video Streaming Requirements

| Specification | Requirement |
| ------------- | ----------- |
| HDMI Version | 2.0
| HDCP Version | 2.2
| Codec | HEVC, VP9
| Encoding Profile | HEVC Level 5.1 Main 10 or VP9 Profile 0
| Input Framerate | 24p, 25p, 30p, 50p, 60p
| Color Space | Rec.709, Rec.2020
| Key Frame Intervals | < 10s (VOD), < 5s (Live)
| Resolution | 3840x2160
| Encryption | PlayReady only

**Additional Notes:**

* `DASH` is the preferred adaptive streaming format with `HEVC` as the preferred video codec.
* The recommended bit rates for variants in an adaptive bit rate stream are 8, 10, 15, and 20Mbps.
* All recommended bit rates should be in the same `DASH` stream.
* All encoded segments must start with an IDR frame.
* The recommended adaptive streaming segment sizes are 2.5, 3.3, or 5 seconds.
* The segment sizes should be constant and the same for all bit rates for the best adaptive switching and trick play (with 10 second BIF files).

#### Detecting 4K UHD Compatibility

There are several conditions that must be checked to see if 4K UHD content can be played:

* Video output mode must be 2160p
* HDCP 2.2 must be enabled
* The device must be able to decode the proper codecs and encoding profiles
* (Optional) Check if the device decrypts within a TEE

```brightscript
Function CanPlay4K() as Boolean
  dev_info = CreateObject("roDeviceInfo")
  hdmi_status = CreateObject("roHdmiStatus")

  ' Check if the output mode is 2160p
  video_mode = dev_info.GetVideoMode()
  if (Left(video_mode, 5) <> "2160p")
    return false 'output mode is not set to 2160p
  end if

  ' Check if HDCP 2.2 is enabled
  if hdmi_status.IsHdcpActive("2.2") <> true
    return false 'HDCP version is not 2.2
  end if

  ' Check if the Roku player can decode 4K 60fps HEVC streams or 4K 30fps vp9 streams
  hevc_video = { Codec: "hevc", Profile: "main", Level: "5.1" }
  vp9_video = { Codec: "vp9", Profile: "profile 0" }
  can_decode_hevc = dev_info.CanDecodeVideo(hevc_video)
  can_decode_vp9 = dev_info.CanDecodeVideo(vp9_video)
  if can_decode_hevc.result <> true OR can_decode_vp9.result <> true
    return false 'device cannot decode 4K HEVC AND VP9 streams
  end if

  ' (Optional) Check if the Roku player decrypts inside a TEE
  drm_info = dev_info.GetDrmInfo()
  if Instr(1, drm_info.playready, "tee") = 0
    return false 'device does not decrypt inside TEE
  end if

  return true
End Function
```

> :information_source: This example returns true only if both 4K HEVC and 4K VP9 decoding is supported. If your 4K UHD content is only encoded in one of these codecs, modify the third conditional statement as necessary.

### HDR10 Video Streaming Requirements

| Specification | Requirement |
| ------------- | ----------- |
| HDMI Version | 2.0a
| HDCP Version | 2.2
| Codec | HEVC, VP9
| Encoding Profile | HEVC Level 5.1 Main 10, VP9 Profile 2
| Input Framerate | 24p, 25p, 30p, 50p, 60p
| Color Space | Rec.2020

#### Detecting HDR10 Compatibility

Before playback, check to see if the Roku device and connected display supports HDR10 with GetDisplayProperties().hdr10 using the [roDeviceInfo](https://sdkdocs.roku.com/display/sdkdoc/ifDeviceInfo) component.

```brightscript
Function canPlayHDR() as Boolean
  dev_info = createObject("roDeviceInfo")
  return dev_info.getDisplayProperties().hdr10
End Function
```

> :information_source: This function should only be called after detecting [4K UHD Compatibility](#detecting-4k-uhd-compatibility).

**Additional Notes:**

* HDR10 is only supported on Roku Premiere Plus and Roku Ultra at this time.
* Adaptive bitrate streams should not have HDR and non-HDR variants in the same manifest.
