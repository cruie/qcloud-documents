## API Name
ConcatVideo

## Feature Description
1. Stitch multiple video files into a new video file, and add it to VOD system. The newly generated video file will have a new FileID;
1. Terminology: the video file to be stitched is the "source file", the new file generated after stitching is the "target file";
1. Not all video files can be stitched. For the requirements of source file, refer to [Stitching Conditions of Source File](#.E6.BA.90.E6.96.87.E4.BB.B6.E5.8F.AF.E6.8B.BC.E6.8E.A5.E6.9D.A1.E4.BB.B6);
1. For HLS stitching, refer to [HLS Video Stitching Instruction](#hls.E8.A7.86.E9.A2.91.E6.8B.BC.E6.8E.A5.E8.AF.B4.E6.98.8E);
1. For the output format of target file, refer to [Target File Output Format Instruction](#.E7.9B.AE.E6.A0.87.E6.96.87.E4.BB.B6.E8.BE.93.E5.87.BA.E6.A0.BC.E5.BC.8F.E8.AF.B4.E6.98.8E);
1. For **users who transfer LVB/ILVB recording to VOD**, if you **stitch multiple recorded video clips for the same LVB**, this function is generally able to meet your stitching input requirements.

### Stitching Conditions of Source File

1. The source file supports MP4/FLV/HLS encapsulation format. For the stitching of HLS videos generated by LVB/ILVB recording, refer to [HLS Video Stitching Instruction](#hls.E8.A7.86.E9.A2.91.E6.8B.BC.E6.8E.A5.E8.AF.B4.E6.98.8E);
1. The total number of source files shall not exceed 500. Note: If HLS stitching is involved, refer to [HLS Video Stitching Instruction](#hls.E8.A7.86.E9.A2.91.E6.8B.BC.E6.8E.A5.E8.AF.B4.E6.98.8E);
1. The total size of source files shall not exceed 10G;
1. The encapsulation format of source files **must be the same**. For example, the format must all be FLV, MP4, or HLS;
1. The audio/video encoding format of source files **must be the same**. For example, the video encoding format must all be H.264 or H.265;
1. The resolution of source files **must be the same**;
1. The colorimetric space and time base of source files **must be the same**;
1. The bit rate and frame rate of source files can be different.

### HLS Video Stitching Instruction

1. HLS stitching is not a simple combination of M3U8 files, but will draw out all the ts files for stitching;
1. For HLS stitching, the "source file" refers to the ts file contained in M3U8. That is, all the TS slices of the HLS video to be stitched must meet the [Stitching Conditions of Source File](#.E6.BA.90.E6.96.87.E4.BB.B6.E5.8F.AF.E6.8B.BC.E6.8E.A5.E6.9D.A1.E4.BB.B6).

### Target File Output Format Instruction

1. The output format of the target file only supports MP4/HLS;
1. The audio/video encoding format of the target file must be the same as the source files;
1. The resolution of the target file must be the same as the source files;
1. The colorimetric space and time base of the target file must be the same as the source files;
1. The bit rate and frame rate of the target file must be the same as each source file. That is, the bit rate and frame rate of a clip of the target file must be the same as its source file.

## Event Notification
Completion of file upload can trigger [Event Notification - Video Stitching Completed](/document/product/266/7834). It can be used by the APP backend server to monitor the video stitching action in the VOD system.

For details, refer to [Introduction to Server Event Notification](/document/product/266/7829).

### Request Domain
vod.api.qcloud.com

### Peak Calling Frequency
100 counts/min

##### Parameter Description
| Parameter Name | Required | Type | Description |
|---------------|----------|---------|---------|
| srcFileList.n.fileId | Yes | String | FileID of the source file |
| name          | Yes | String    | Target file name |
| dstType.n      | Yes | String    | Target file output format, support two output formats, mp4 and hls at the same time. Please enter mp4 or m3u8 |
| COMMON_PARAMS | Yes |  | Refer to [Common Parameters](/document/product/266/7782#.E5.85.AC.E5.85.B1.E5.8F.82.E6.95.B0) |

### Request Example
```
https://vod.api.qcloud.com/v2/index.php?Action=ConcatVideo
&srcFileList.0.fileId=16092504232103571364
&srcFileList.1.fileId=16092504232103571365
&name=testfile
&dstType.0=m3u8
&COMMON_PARAMS
```

## API Response

##### Parameter Description
| Name | Type | Description |
|---------|---------|---------|
| code | Integer | Error code, 0:  Succeeded, other values:  Failed |
| message | String | Error message |
| vodTaskId | String | Unique id to describe the stitching task, which can be used to query the task status |

### Error Code Description
| Error Code | Description |
|---------|---------|
| 4000-7000 | Refer to [Common Error Codes](/document/product/266/7783)  |
| 1000 | Invalid parameter  |
| 1001 | User information error  |
| 10009 | Unexpected file status  |

### Response Example

```javascript
{
    "code": 0,
    "message": "",
    "vodTaskId": ""
}
```

