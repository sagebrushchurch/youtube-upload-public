# Youtube-Upload

Command-line script to upload videos to Youtube using the Youtube [APIv3](https://developers.google.com/youtube/v3/).

## Setup

The Youtube API uses [OAuth 2.0](https://developers.google.com/accounts/docs/OAuth2) to authenticate the upload.

> [!IMPORTANT]
> The first time you try to upload a video, you will be asked to follow a URL in your browser to get an authentication token.
> If you have multiple channels for the logged in user, you will also be asked to pick which one you want to upload the videos to.

You now must create and use [your own OAuth 2.0 file](https://developers.google.com/youtube/registering_an_application).

<details>
  <summary>Click here if you need to create one.</summary>

* Go to the Google [console](https://console.developers.google.com/).
* _Create project_.
* Side menu: _APIs & auth_ -> _APIs_
* Top menu: _Enabled API(s)_: Enable all Youtube APIs.
* Side menu: _APIs & auth_ -> _Credentials_.
* _Create a Client ID_: Add credentials -> OAuth 2.0 Client ID -> Other -> Name: youtube-upload -> Create -> OK
* _Download JSON_: Under the section "OAuth 2.0 client IDs". Save the file to `~/.client_secrets.json`.
</details>

## Install

> [!TIP]
> Use of the Semaphore playbook is preferred, as it automates the following steps.

<details>
  <summary>Click here if you're a rebel.</summary>

* Install the python dependencies

```
sudo pip3 install --upgrade google-api-python-client oauth2client
```

* Make sure `my_credentials.json` and `.client_secrets.json` are both located in your **home folder**.
</details>

* Set `scriptman.service` with the correct `CAMPUS` environment variable.

```
sudo nano /etc/systemd/system/scriptman.service
```
and set where you see `Environment=CAMPUS=""` enter the name of the campus in the quotes.
Example: `Environment=CAMPUS="Riverside"`

## Examples

### Upload a video

```
youtube-upload \
--title="$CAMPUS Service $(date +%D)" \
--privacy="unlisted" \
--playlist="$CAMPUS" \
/home/pi/service.mp4
```

### Other extra metadata available :

```
--description="A.S. Mutter plays Beethoven"
--thumbnail (string)
--tags="mutter, beethoven"
--privacy (public | unlisted | private)
--category="Music"
--publish-at (YYYY-MM-DDThh:mm:ss.sZ)
--recording-date="2011-03-10T15:32:17.0Z"
--location (latitude=VAL,longitude=VAL[,altitude=VAL])
--default-language="en"
--default-audio-language="en"
--embeddable=True|False
--credentials-file="my_credentials.json"
--client-secrets="my_client_secrets.json"
```

<!-- * Split a video with _ffmpeg_

If your video is too big or too long for Youtube limits, split it before uploading:

```
$ bash examples/split_video_for_youtube.sh video.avi
video.part1.avi
video.part2.avi
video.part3.avi
```
-->