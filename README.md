## GSTV - Dover Integration
## User Experience
- The video and audio will be playing at the dispenser in a continuous manner.
- The video and audio will be synchronized across all screens at a given site.
- Video and audio will be interrupted during the fueling transaction only during payment prompts as required for PCI compliance.
  - After prompts video and audio will resume synchronized with the other terminals.
- The size of the media window will accommodate interactive prompts that drive the fuel purchase transaction.

## Overview
- GSTV will provide a unique playlist for every site every day.
- At any time the Dover system will accept multiple updates to the playlists, assets, and metadata for a site without playback interruptions.
- The video window will be divided into multiple zones
  - Video playback will exist within a single zone

## Global Processing
### File Missing at Terminal
  - Terminal will check Dover cloud for required media
    - **If success**
      - Download required media to terminal
    - **If failure**
      - Dover sends notification to GSTV to resend media update notification
      - GSTV sends media update notification to Dover


#### What Dover Sends if Media File is not in Dover Cloud
1. Dover should retry downloading the file three times.
1. If the download is still unsuccessful, send notification to GSTV

```javascript
{
  "guid": String,
  "id": String,
  "filename": String,
  "notificationTimestamp": String
  "notificationType": "MEDIA_CHECK",
  "status": "failure",
  "additionalInformation": "Video File is Missing - {filename}"
}
```
If update message is playlist, add additional field to notification
```javascript
  "segmentIds": [String]
```

[What Dover Sends if Media File is not in Dover Cloud - Example Data](/gstv-initiated-messages/configuration-site/samples/json/sample1_notificationfailure2.json)

#### What Dover Sends if a Media File Can Not Be Downloaded to a Terminal and is not in the Dover Cloud
```javascript
{
  "guid": String,
  "id": String,
  "filename": String,
  "notificationTimestamp": String,
  "notificationType": "MEDIA_CHECK",
  "siteId": String,
  "status": "failure",
  "terminalId": String,
  "additionalInformation": "Video File is Missing - {filename}"
}
```

### Update Notification Processing Order
- Media update notifications should be processed before playlist update notifications
  - If a playlist update is being processed when more media updates arrive, processing of the playlist should be completed prior to media updates
- Configuration changes are processed as they arrive, but site playback should not be interrupted
