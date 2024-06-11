# Video Player Component

This Vue component, `VideoPlayer`, is designed to handle both HTML5 and YouTube videos. It supports autoplay, pause, and play functionalities, and can detect scroll positions to show or hide a smaller version of the video when the main video is not visible on the screen.

## Features

- **Autoplay**: The video will automatically play when the component is mounted.
- **Pause and Play**: Control the video playback.
- **Scroll Detection**: Displays a smaller version of the video when the main video is out of view.

## Props

- `thumb` (String): The URL of the thumbnail image for the video.
- `thumbWidth` (Number): The width of the thumbnail image.
- `thumbHeight` (Number): The height of the thumbnail image.
- `thumbAlt` (String): The alt text for the thumbnail image.
- `videoWidth` (Number): The width of the video player.
- `videoHeight` (Number): The height of the video player.
- `removemargin` (String, default: `null`): Class to remove margin from the container.
- `imgSec` (Boolean, default: `true`): Flag to determine if the image section should be displayed.
- `videoType` (String, default: `'html5'`): The type of video player to use (`'html5'` or `'youtube'`).
- `video` (String): The URL or YouTube ID of the video.

## Setup

1. Import the component into your Vue project.
2. Pass the necessary props to the component.
