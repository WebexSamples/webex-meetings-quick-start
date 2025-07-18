# 🚀 Webex Javascript SDK Meetings Quick Start

This sample application demonstrates the basic usage of the Webex Javascript SDK. It allows you to create a Webex meeting using the meetings plugin, providing a simple and straightforward introduction to building video calling applications with the Webex platform.

## ✨ Features

- **🎯 Quick SDK Integration** - Minimal setup to get Webex meetings running
- **🎥 Video Calling** - Full video and audio communication capabilities
- **📧 Multiple Destinations** - Support for Person ID, email, SIP URI, and Room ID
- **🖥️ Self & Remote Views** - Local and remote video stream display
- **🎙️ Media Controls** - Camera and microphone stream management
- **⚡ Parcel Build System** - Fast development server with hot reloading
- **📱 Browser-based** - No plugins required, works directly in modern browsers

## 🚀 Setup

### Prerequisites

> [!IMPORTANT]
> You need to be on **Node V18** or later. If not, `yarn install` will throw an error stating the same.

- Node.js 18.0.0 or later
- Yarn package manager
- Valid Webex access token from [developer.webex.com](https://developer.webex.com)

### Clone the repo to your local machine

```bash
git clone git@github.com:WebexSamples/webex-meetings-quick-start.git
cd webex-meetings-quick-start
```

### Install Dependencies

```bash
yarn install
```

### Add Personal Access Token

For this example, we'll use your personal access token which can be found in [Getting Started](https://developer.webex.com/docs/api/getting-started#accounts-and-authentication) if you're logged in. This token provides access to your account for testing purposes, and shouldn't be used for anything other than testing. If you don't already have a Webex Teams account, click Sign Up at the top of this page to create a new account.

Open the [`index.js`](index.js) file and add this code to authenticate with Webex Teams, replacing `YOUR_ACCESS_TOKEN` with your development token:

```javascript
// Change to your access token from developer.webex.com
const myAccessToken = 'YOUR_ACTUAL_ACCESS_TOKEN';
```

### Start Server

In this example we'll use [Parcel](https://parceljs.org/) to build and serve our web app, including the SDK.

Let's serve the web app with Parcel and start a meeting in the browser!

```bash
yarn start
```

Open the app ([http://localhost:1234](http://localhost:1234/)) in your browser to use your new web app! 
Enter the Person ID or email address of who you want to start a meeting with and click the `join` button. 
Congratulations, you've made your first call in the browser using the Webex Browser SDK!

## 📖 Usage Guide

### Getting Started

1. **Launch the Application:**
   - Start the development server with `yarn start`
   - Open [http://localhost:1234](http://localhost:1234) in your browser

2. **Join a Meeting:**
   - Enter a destination in the input field
   - Click the "join" button to start the meeting
   - Grant camera and microphone permissions when prompted

3. **During the Meeting:**
   - Your video appears in the left panel (self-view)
   - Remote participant video appears in the right panel
   - Use the "cancel/hangup" button to end the meeting

### Supported Destinations

| Destination Type | Format | Example |
|------------------|--------|---------|
| **Email Address** | user@domain.com | john.doe@company.com |
| **Person ID** | Webex Person ID | Y2lzY29zcGFyazovL3VzL1BFT1BMRS... |
| **SIP URI** | SIP address | meeting@example.webex.com |
| **Room ID** | Webex Room ID | Y2lzY29zcGFyazovL3VzL1JPT00v... |

### Basic Operations

```javascript
// Create a meeting
const meeting = await webex.meetings.create(destination);

// Join with media streams
await meeting.joinWithMedia(meetingOptions);

// Leave the meeting
meeting.leave();
```

## 🏗️ Project Structure

```
webex-meetings-quick-start/
├── index.html              # Main HTML template with video elements
├── index.js                # Core application logic and SDK integration
├── package.json            # Dependencies and build scripts
├── yarn.lock               # Yarn dependency lock file
├── .npmrc                  # npm configuration (engine-strict)
├── .gitignore              # Git ignore patterns
├── LICENSE                 # Cisco Sample Code License
└── README.md               # This documentation
```

### Core Files

| File | Description | Purpose |
|------|-------------|---------|
| **index.html** | HTML template with video elements | UI structure and media containers |
| **index.js** | Main application logic | SDK initialization, meeting creation, and media handling |
| **package.json** | Project configuration | Dependencies, scripts, and Node.js requirements |

## 🔧 Code Walkthrough

### SDK Initialization

```javascript
const Webex = require('webex');

const webex = Webex.init({
  credentials: {
    access_token: myAccessToken
  }
});

// Register for meetings functionality
webex.meetings.register();
```

### Meeting Creation and Join

```javascript
// Create meeting with destination
const meeting = await webex.meetings.create(destination);

// Create media streams
const microphoneStream = await webex.meetings.mediaHelpers.createMicrophoneStream({
  echoCancellation: true,
  noiseSuppression: true,
});

const cameraStream = await webex.meetings.mediaHelpers.createCameraStream({ 
  width: 640, 
  height: 480 
});

// Join with media options
const meetingOptions = {
  mediaOptions: {
    allowMediaInLobby: true,
    shareAudioEnabled: false,
    shareVideoEnabled: false,
    localStreams: {
      camera: cameraStream,
      microphone: microphoneStream
    },      
  },
};

await meeting.joinWithMedia(meetingOptions);
```

### Media Event Handling

```javascript
function bindMeetingEvents(meeting) {
  // Handle remote media streams
  meeting.on('media:ready', (media) => {
    if (media.type === 'remoteVideo') {
      document.getElementById('remote-view-video').srcObject = media.stream;
    }
    if (media.type === 'remoteAudio') {
      document.getElementById('remote-view-audio').srcObject = media.stream;
    }
  });

  // Handle media stream stopping
  meeting.on('media:stopped', (media) => {
    if (media.type === 'remoteVideo') {
      document.getElementById('remote-view-video').srcObject = null;
    }
    if (media.type === 'remoteAudio') {
      document.getElementById('remote-view-audio').srcObject = null;
    }
  });

  // Handle meeting errors
  meeting.on('error', (err) => {
    console.error(err);
  });
}
```

### HTML Structure

```html
<!-- Input form for meeting destination -->
<form id="destination">
  <input
    id="invitee"
    placeholder="Person ID or Email Address or SIP URI or Room ID"
    type="text">
  <input title="join" type="submit" value="join">
</form>

<!-- Video containers -->
<div style="display: flex">
  <video id="self-view" muted autoplay playsinline></video>
  <div>
    <audio id="remote-view-audio" autoplay></audio>
    <video id="remote-view-video" autoplay playsinline></video>
  </div>
</div>

<!-- Meeting controls -->
<button id="hangup" type="button">cancel/hangup</button>
```

## ⚙️ Configuration

### Package Dependencies

```json
{
  "dependencies": {
    "webex": "3.1.0"
  },
  "engines": {
    "node": ">=18.0.0"
  },
  "devDependencies": {
    "parcel": "^2.12.0",
    "crypto-browserify": "^3.12.0",
    // ... additional browserify polyfills
  }
}
```

### Media Stream Options

```javascript
// Microphone configuration
const microphoneStream = await webex.meetings.mediaHelpers.createMicrophoneStream({
  echoCancellation: true,    // Enable echo cancellation
  noiseSuppression: true,    // Enable noise suppression
});

// Camera configuration  
const cameraStream = await webex.meetings.mediaHelpers.createCameraStream({ 
  width: 640,               // Video width
  height: 480               // Video height
});
```

### Meeting Options

```javascript
const meetingOptions = {
  mediaOptions: {
    allowMediaInLobby: true,      // Allow media before meeting starts
    shareAudioEnabled: false,     // Disable audio sharing
    shareVideoEnabled: false,     // Disable video sharing
    localStreams: {
      camera: cameraStream,       // Local camera stream
      microphone: microphoneStream // Local microphone stream
    },      
  },
};
```

## 🧪 Testing

### Manual Testing Steps

1. **Basic Functionality:**
   - Start the application and verify UI loads correctly
   - Test with different destination formats
   - Verify camera and microphone permissions

2. **Meeting Flow:**
   - Create a meeting with a valid destination
   - Check that self-view displays local video
   - Verify remote media streams when another participant joins
   - Test hang up functionality

3. **Error Handling:**
   - Test with invalid destinations
   - Verify error messages display correctly
   - Check behavior when media permissions are denied

### Browser Compatibility

| Browser | Version | Support Status |
|---------|---------|----------------|
| Chrome | 90+ | ✅ Full Support |
| Firefox | 88+ | ✅ Full Support |
| Safari | 14+ | ✅ Full Support |
| Edge | 90+ | ✅ Full Support |

## 🚨 Troubleshooting

### Common Issues

| Issue | Solution |
|-------|----------|
| **Node version error** | Ensure you're using Node.js 18.0.0 or later |
| **Access token error** | Replace `YOUR_ACCESS_TOKEN` with your actual token |
| **Media permissions denied** | Grant camera/microphone permissions in browser |
| **Meeting creation fails** | Verify destination format and network connectivity |
| **Video not displaying** | Check browser WebRTC support and media constraints |

### Debug Mode

The application includes debug logging enabled by default:

```javascript
webex.config.logger.level = 'debug';
```

Check the browser console for detailed SDK logs and error messages.

### Common Fixes

```javascript
// Check if access token is properly set
if (myAccessToken === 'YOUR_ACCESS_TOKEN') {
  alert('Make sure to update your access token in the index.js file!');
  throw new Error('Make sure to update your access token in the index.js file!');
}

// Verify meeting registration
webex.meetings.register()
  .catch((err) => {
    console.error(err);
    alert(err);
    throw err;
  });
```

## 📚 Additional Resources

To see the full example code we have used here to start a meeting, check out the [sample app in GitHub](https://github.com/webex/webex-js-sdk/tree/master/packages/node_modules/samples/browser-single-party-call).

### Documentation Links

- [Webex JavaScript SDK Documentation](https://webex.github.io/webex-js-sdk/)
- [Webex Meetings API Guide](https://developer.webex.com/docs/meetings)
- [Getting Started with Webex APIs](https://developer.webex.com/docs/api/getting-started)
- [Webex Developer Portal](https://developer.webex.com)

### Related Samples

- [Browser Single Party Call](https://github.com/webex/webex-js-sdk/tree/master/packages/node_modules/samples/browser-single-party-call)
- [Webex Meeting Widget Starter](../webex-meeting-widget-starter)
- [Webex JS SDK Calling Demo](../webex-js-sdk-calling-demo)

## 🤝 Contributing

We truly appreciate your contribution to the Webex Samples!

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/meeting-enhancement`
3. Commit changes: `git commit -am 'Add meeting feature'`
4. Push to branch: `git push origin feature/meeting-enhancement`
5. Submit a Pull Request

### Development Guidelines

- Follow the existing code style and patterns
- Test with multiple browsers and destinations
- Update documentation for new features
- Ensure Node.js 18+ compatibility

## 📄 License

This project is licensed under the Cisco Sample Code License - see the [LICENSE](LICENSE) file for details.

## 🆘 Support

For technical support and questions:

- **Issues**: Submit via GitHub Issues
- **SDK Documentation**: [Webex JavaScript SDK](https://webex.github.io/webex-js-sdk/)
- **API Documentation**: [Webex Developer Portal](https://developer.webex.com)
- **Community**: [Webex Developer Community](https://developer.webex.com/community)

## Thanks!

Made with ❤️ by the Webex Developer Relations Team at Cisco

---

**Note**: This quick start is designed for development and testing purposes. For production applications, implement additional security measures, error handling, and user experience optimizations.
