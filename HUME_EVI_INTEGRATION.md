# Hume EVI Integration Plan for Bonzai

## Executive Summary

This document outlines the integration strategy for Hume's Empathic Voice Interface (EVI) into the Bonzai platform, enabling Zai to communicate via voice with emotional intelligence.

## What is Hume EVI?

**Hume EVI (Empathic Voice Interface)** is a conversational voice AI API with emotional intelligence capabilities:

- **Speech-to-Speech AI**: Real-time voice conversations
- **Emotional Intelligence**: Detects and responds to user emotions
- **Low Latency**: Optimized for real-time interactions
- **Empathic Responses**: Adjusts tone and delivery based on emotional context

## Technical Architecture

### Connection Protocol
- **Primary**: WebSocket for real-time bidirectional audio streaming
- **Alternative**: Server-Sent Events (SSE) for specific use cases
- **Authentication**: API key + client secret (token-based)

### Audio Specifications
- **Input**: Browser MediaStream API (microphone capture)
- **Output**: Browser Web Audio API (speaker playback)
- **Format**: Real-time audio chunks over WebSocket
- **Latency**: Optimized for conversational experiences

### API Structure
```
Authentication Flow:
1. Fetch access token using API key + client secret
2. Establish WebSocket connection with token
3. Begin audio streaming
4. Receive transcripts + audio responses in real-time
```

## Integration Approaches for Bonzai

### Approach 1: React SDK Integration (RECOMMENDED)

**Package**: `@humeai/voice-react`

**Advantages:**
- Official React hooks for EVI
- Pre-built audio handling
- WebSocket management included
- Emotion detection built-in
- Minimal setup required

**Implementation Pattern:**
```typescript
import { useVoice } from '@humeai/voice-react'

function VoiceInterface() {
  const { connect, disconnect, status, messages } = useVoice({
    apiKey: process.env.HUME_API_KEY,
    clientSecret: process.env.HUME_CLIENT_SECRET
  })

  // Audio automatically handled by SDK
  // Messages contain transcripts + emotional data
}
```

### Approach 2: TypeScript SDK Integration

**Package**: `@humeai/hume-typescript-sdk`

**Advantages:**
- More control over implementation
- Works with Electron/Tauri main process
- Can integrate with existing WebSocket infrastructure
- Suitable for desktop environments

**Implementation Pattern:**
```typescript
import { HumeClient } from '@humeai/hume-typescript-sdk'

const client = new HumeClient({ apiKey: 'YOUR_KEY' })

// Establish WebSocket connection
// Handle audio streaming manually
// Process transcripts and responses
```

### Approach 3: Embedded Widget

**Package**: `@humeai/voice-embed-react`

**Advantages:**
- Fastest to implement
- UI included
- Drop-in component

**Disadvantages:**
- Less customization
- May not match Bonzai's design system
- Less integration with Zai's personality

## Recommended Architecture for Bonzai

### Component Structure

```
src/
├── components/
│   ├── Voice/
│   │   ├── VoiceInterface.tsx      # Main voice UI component
│   │   ├── VoiceButton.tsx         # Mic/mute button
│   │   ├── VoiceWaveform.tsx       # Audio visualization
│   │   ├── VoiceStatus.tsx         # Connection status
│   │   └── VoiceControls.tsx       # PTT, volume, etc.
│   └── ...
├── hooks/
│   ├── useVoiceConnection.ts       # WebSocket management
│   ├── useAudioCapture.ts          # Microphone handling
│   ├── useAudioPlayback.ts         # Speaker output
│   └── useVoiceState.ts            # Voice UI state
├── services/
│   ├── hume/
│   │   ├── HumeClient.ts           # API wrapper
│   │   ├── auth.ts                 # Token management
│   │   ├── audio.ts                # Audio processing
│   │   └── types.ts                # TypeScript types
│   └── ...
└── atoms/
    └── voiceState.ts                # Jotai voice state atoms
```

### State Management (Jotai)

```typescript
// src/atoms/voiceState.ts
import { atom } from 'jotai'

export type VoiceStatus = 'idle' | 'connecting' | 'connected' | 'speaking' | 'listening' | 'error'

export const voiceStatusAtom = atom<VoiceStatus>('idle')
export const voiceMessagesAtom = atom<VoiceMessage[]>([])
export const voiceEnabledAtom = atom<boolean>(false)
export const voiceMutedAtom = atom<boolean>(false)
export const voiceVolumeAtom = atom<number>(1.0)
```

### Integration with Existing Chat

**Dual Mode Interface:**
1. **Text Mode** (default): Current chat interface
2. **Voice Mode** (toggle): Hume EVI active

**Implementation:**
- Voice transcripts feed into chat history
- Text responses can be spoken via EVI TTS
- Seamless switching between modes
- Voice and text share same message history

## Step-by-Step Implementation Plan

### Phase 1: Setup & Authentication (1-2 hours)
1. Install Hume React SDK: `npm install @humeai/voice-react`
2. Add environment variables for API keys
3. Create Hume client wrapper service
4. Implement token authentication flow
5. Test connection establishment

### Phase 2: Audio Infrastructure (2-3 hours)
1. Create microphone capture hook (`useAudioCapture`)
2. Implement audio playback hook (`useAudioPlayback`)
3. Add browser permission handling
4. Build audio visualization component
5. Test audio I/O functionality

### Phase 3: Voice UI Components (2-3 hours)
1. Create `VoiceInterface` main component
2. Build voice control buttons (mute, PTT, volume)
3. Add connection status indicator
4. Implement waveform visualization
5. Style components to match Bonzai design

### Phase 4: State Management (1-2 hours)
1. Create voice state atoms
2. Integrate with existing chat state
3. Sync voice transcripts to message history
4. Handle voice/text mode switching
5. Persist voice preferences

### Phase 5: Integration with Zai (2-3 hours)
1. Connect voice input to LLM processing
2. Route LLM responses through EVI TTS
3. Apply Zai personality to voice interactions
4. Integrate with MCP tools (voice-triggered)
5. Test end-to-end flow

### Phase 6: Polish & Error Handling (1-2 hours)
1. Add graceful error handling
2. Implement reconnection logic
3. Handle network interruptions
4. Add loading states
5. Test edge cases

**Total Estimated Time:** 10-15 hours

## UI/UX Considerations

### Voice Button Placement
**Option A:** Toggle button in chat input area (next to send button)
**Option B:** Persistent voice button overlay (bottom-right corner)
**Option C:** Keyboard shortcut triggered (e.g., Cmd/Ctrl + Shift + V)

**Recommendation:** Option A + C for flexibility

### Visual Feedback
- **Idle**: Gray microphone icon
- **Listening**: Pulsing blue waveform
- **Speaking** (Zai): Purple/animated waveform
- **Muted**: Red slash through mic
- **Error**: Yellow warning indicator

### Accessibility
- Keyboard shortcuts for all voice controls
- Screen reader announcements for status changes
- Visual indicators for audio levels
- Fallback to text if voice unavailable

## Configuration Structure

```json
// .config/voice_config.json
{
  "hume": {
    "apiKey": "your_hume_api_key",
    "clientSecret": "your_hume_client_secret",
    "configId": "optional_evi_config_id"
  },
  "voice": {
    "defaultEnabled": false,
    "pushToTalk": false,
    "autoSpeak": true,
    "volume": 1.0,
    "micSensitivity": 0.5
  },
  "wakeWord": {
    "enabled": false,
    "phrase": "hey zai"
  }
}
```

## Security Considerations

### API Key Management
- **DO NOT** commit API keys to git
- Store in `.env` files (gitignored)
- Use environment variables in production
- Consider Electron/Tauri secure storage for keys

### Permissions
- Request microphone permission explicitly
- Provide clear explanation of voice usage
- Allow users to revoke permissions
- Handle permission denials gracefully

### Privacy
- Audio processed in real-time (not stored by default)
- Hume EVI privacy policy: [Link needed from Hume docs]
- Option to disable cloud processing (local-only mode)
- Clear data retention policies

## Testing Strategy

### Unit Tests
- Audio capture/playback functions
- State management (atoms)
- WebSocket connection handling
- Error scenarios

### Integration Tests
- Voice → LLM → Response flow
- MCP tool triggering via voice
- Text/voice mode switching
- Reconnection scenarios

### Manual Testing
- Various microphone types
- Different network conditions
- Background noise handling
- Interruption handling

## Performance Optimization

### Latency Reduction
- Preload WebSocket connection
- Keep connection alive with heartbeat
- Use audio buffer optimization
- Minimize state updates during streaming

### Resource Management
- Close audio streams when not in use
- Cleanup WebSocket on disconnect
- Throttle visualization updates
- Lazy load voice components

## Fallback Strategies

### No Microphone Access
- Fall back to text input
- Show clear message to user
- Provide troubleshooting steps

### Network Issues
- Automatic reconnection with exponential backoff
- Queue voice inputs during disconnection
- Show connection status clearly

### API Errors
- Display user-friendly error messages
- Log technical details for debugging
- Provide manual reconnect option

## Dependencies to Add

```json
{
  "dependencies": {
    "@humeai/voice-react": "^latest",
    "@humeai/hume-typescript-sdk": "^latest"
  },
  "devDependencies": {
    "@types/dom-mediacapture-record": "^latest"
  }
}
```

## Environment Variables Required

```bash
# .env
HUME_API_KEY=your_api_key_here
HUME_CLIENT_SECRET=your_client_secret_here
HUME_CONFIG_ID=optional_evi_configuration_id
```

## Resources & References

### Official Documentation
- Hume EVI Overview: https://dev.hume.ai/docs/empathic-voice-interface-evi/overview
- Hume API Reference: https://dev.hume.ai/reference/empathic-voice-interface-evi/chat/chat
- React SDK: https://github.com/HumeAI/hume-react-sdk
- TypeScript Examples: https://github.com/HumeAI/hume-evi-typescript-example
- API Examples Repository: https://github.com/HumeAI/hume-api-examples

### Key Examples to Study
- **TypeScript Quickstart**: `/evi/evi-typescript-quickstart/`
- **React Native** (similar to Electron): `/evi/evi-react-native/`
- **Next.js Integration**: `/evi/evi-next-js-app-router-quickstart/`
- **Function Calling**: `/evi/evi-next-js-function-calling/`

### Community Resources
- Hume Discord: https://hume.ai/discord
- GitHub Discussions: https://github.com/HumeAI/hume-api-examples/discussions

## Next Steps

1. **Obtain Hume API Credentials**
   - Sign up at https://platform.hume.ai/
   - Generate API key and client secret
   - Create EVI configuration (optional)

2. **Set Up Development Environment**
   - Install dependencies
   - Configure environment variables
   - Test basic connection

3. **Prototype Voice Button**
   - Build minimal UI
   - Establish WebSocket connection
   - Test audio capture/playback

4. **Iterate and Expand**
   - Add features incrementally
   - Test with real usage
   - Gather feedback

## Risks & Mitigation

| Risk | Mitigation |
|------|-----------|
| High latency | Use WebSocket, optimize buffer sizes |
| Audio quality issues | Test across devices, add quality settings |
| Browser compatibility | Feature detection, clear requirements |
| API rate limits | Implement request throttling, monitor usage |
| Cost overruns | Set up usage alerts, implement usage caps |

## Success Metrics

- **Voice activation time**: < 500ms from button press to listening
- **Response latency**: < 2s from voice input to Zai response
- **Accuracy**: > 95% transcript accuracy in quiet environments
- **Reliability**: > 99% successful connection rate
- **User adoption**: > 30% of users try voice mode

## Conclusion

Integrating Hume EVI into Bonzai is achievable with the React SDK and existing architecture. The estimated 10-15 hour implementation provides voice-enabled Zai interactions with emotional intelligence, aligning perfectly with the Bonzai vision of a multi-modal AI orchestration platform.

**Recommendation:** Start with React SDK (`@humeai/voice-react`) for fastest path to working prototype, then optimize based on user feedback and performance needs.
