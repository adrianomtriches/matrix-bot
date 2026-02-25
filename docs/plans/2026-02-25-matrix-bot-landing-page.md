# Matrix Bot Landing Page Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a Matrix-themed landing page with animated digital rain background and a live chat widget for the "Matrix Bot" WhatsApp AI agent business.

**Architecture:** Single self-contained HTML file. Canvas API renders the Matrix rain background. CSS handles all animations (glow, pulse, typing). JavaScript manages chat widget state with pre-scripted mock responses. No frameworks, no dependencies.

**Tech Stack:** HTML5 Canvas, vanilla CSS3 (animations, flexbox, grid), vanilla JavaScript (ES6+)

---

### Task 1: Scaffold HTML + Matrix Rain Background

**Files:**
- Create: `index.html`

**Step 1: Create the base HTML structure with Canvas**

Create `index.html` with:
- HTML boilerplate, viewport meta, title "Matrix Bot"
- Full-screen `<canvas id="matrix-rain">` element
- Basic CSS reset: margin 0, padding 0, overflow hidden, black background, font-family monospace
- Canvas positioned fixed, top 0, left 0, width/height 100%, z-index 0

**Step 2: Implement the Matrix rain animation**

JavaScript at bottom of file:
- Character set: Katakana (アァカサタナハマヤャラワガザダバパイィキシチニヒミリヰギジヂビピウゥクスツヌフムユュルグズブヅプエェケセテネヘメレヱゲゼデベペオォコソトノホモヨョロヲゴゾドボポヴッン) + Latin uppercase + digits 0-9
- fontSize = 16, columns = canvas.width / fontSize
- rainDrops array initialized with random starting positions (not all at 1 — randomize for immediate visual)
- draw() function:
  - Fill canvas with `rgba(0, 0, 0, 0.05)` for trail fade
  - Set fill to `#0F0`, font to `${fontSize}px monospace`
  - Loop columns, draw random char at `(i * fontSize, rainDrops[i] * fontSize)`
  - Reset drop when past canvas height with `Math.random() > 0.975`
- `setInterval(draw, 30)`
- Handle `window.resize` to update canvas dimensions and recalculate columns

**Step 3: Verify in browser**

Run: `open index.html` (macOS)
Expected: Full-screen Matrix digital rain animation, green characters falling on black background, smooth trail fade effect.

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: scaffold HTML with Matrix rain canvas animation"
```

---

### Task 2: Hero Section

**Files:**
- Modify: `index.html`

**Step 1: Add hero HTML overlay**

Add a `<div id="content">` wrapper (z-index 1, position relative, pointer-events none on wrapper, pointer-events auto on interactive children) containing:

```html
<section id="hero">
  <h1 class="logo">Matrix Bot</h1>
  <p class="tagline">AI-Powered WhatsApp Agents for Your Business</p>
  <button id="chat-toggle" class="cta-button">Talk to our Agent</button>
</section>
```

**Step 2: Style the hero section**

CSS for hero:
- `#content`: position relative, z-index 1, min-height 100vh
- `#hero`: display flex, flex-direction column, align-items center, justify-content center, min-height 100vh, text-align center
- `.logo`: font-size 4rem, color #0F0, font-family 'Courier New' monospace, text-shadow with multiple layers for glow effect: `0 0 10px #0F0, 0 0 20px #0F0, 0 0 40px #0F0, 0 0 80px #003300`. Add CSS animation `glow-pulse` that oscillates text-shadow intensity over 2s ease-in-out infinite
- `.tagline`: color #00CC00, font-size 1.3rem, margin-top 1rem, letter-spacing 2px, opacity 0.9
- `.cta-button`: background transparent, border 2px solid #0F0, color #0F0, padding 15px 40px, font-size 1.1rem, font-family monospace, cursor pointer, text-transform uppercase, letter-spacing 3px, transition all 0.3s. On hover: background rgba(0,255,0,0.1), box-shadow 0 0 20px rgba(0,255,0,0.3)

**Step 3: Verify in browser**

Refresh the page.
Expected: Centered "Matrix Bot" title with green glow pulse, tagline below, CTA button with hover glow effect. Rain visible behind everything.

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add hero section with glowing logo and CTA"
```

---

### Task 3: Chat Widget Structure + Styling

**Files:**
- Modify: `index.html`

**Step 1: Add chat widget HTML**

Add after hero section, inside `#content`:

```html
<div id="chat-widget" class="chat-hidden">
  <div class="chat-header">
    <div class="chat-header-info">
      <span class="chat-status-dot"></span>
      <span class="chat-title">Matrix Bot Agent</span>
    </div>
    <button id="chat-close" class="chat-close-btn">&times;</button>
  </div>
  <div id="chat-messages" class="chat-messages">
    <!-- Messages inserted by JS -->
  </div>
  <div class="chat-input-area">
    <input type="text" id="chat-input" placeholder="Type your message..." autocomplete="off" />
    <button id="chat-send" class="chat-send-btn">&#9654;</button>
  </div>
</div>
```

**Step 2: Style the chat widget**

CSS:
- `#chat-widget`: position fixed, bottom 30px, right 30px, width 400px, height 550px, background rgba(0,0,0,0.92), border 1px solid #0F0, border-radius 8px, display flex, flex-direction column, z-index 10, box-shadow 0 0 30px rgba(0,255,0,0.15), transition all 0.3s ease
- `.chat-hidden`: transform translateY(20px), opacity 0, pointer-events none
- `.chat-visible`: transform translateY(0), opacity 1, pointer-events auto
- `.chat-header`: display flex, justify-content space-between, align-items center, padding 15px 20px, border-bottom 1px solid rgba(0,255,0,0.3), background rgba(0,255,0,0.05)
- `.chat-status-dot`: width 8px, height 8px, background #0F0, border-radius 50%, display inline-block, margin-right 10px, animation `pulse-dot 1.5s infinite`
- `.chat-title`: color #0F0, font-size 0.95rem
- `.chat-close-btn`: background none, border none, color #0F0, font-size 1.5rem, cursor pointer
- `.chat-messages`: flex 1, overflow-y auto, padding 15px 20px, display flex, flex-direction column, gap 12px. Custom scrollbar: thin, green thumb on transparent track
- `.chat-input-area`: display flex, padding 12px, border-top 1px solid rgba(0,255,0,0.3), gap 8px
- `#chat-input`: flex 1, background rgba(0,255,0,0.05), border 1px solid rgba(0,255,0,0.3), color #0F0, padding 10px 15px, font-family monospace, font-size 0.9rem, border-radius 4px, outline none. On focus: border-color #0F0, box-shadow 0 0 10px rgba(0,255,0,0.2)
- `.chat-send-btn`: background transparent, border 1px solid #0F0, color #0F0, padding 10px 15px, cursor pointer, border-radius 4px, font-size 1rem. Hover: background rgba(0,255,0,0.1)

**Step 3: Add message bubble styles**

- `.message`: max-width 80%, padding 10px 15px, border-radius 8px, font-size 0.9rem, line-height 1.4, animation fadeIn 0.3s ease
- `.message.bot`: align-self flex-start, background rgba(0,255,0,0.08), border 1px solid rgba(0,255,0,0.2), color #00DD00
- `.message.user`: align-self flex-end, background rgba(0,255,0,0.15), border 1px solid rgba(0,255,0,0.3), color #0F0
- `.typing-indicator`: align-self flex-start, color rgba(0,255,0,0.6), font-style italic, animation flicker 0.5s infinite alternate

**Step 4: Verify in browser**

Temporarily remove `chat-hidden` class to see the widget.
Expected: Dark terminal-style chat panel in bottom-right with green borders, glowing input field, message area with scrollbar.

**Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add chat widget UI with Matrix terminal styling"
```

---

### Task 4: Chat Widget JavaScript Logic

**Files:**
- Modify: `index.html`

**Step 1: Add open/close toggle logic**

JavaScript:
- `chatToggle` button click → toggle `chat-hidden`/`chat-visible` classes on `#chat-widget`
- `chatClose` button click → add `chat-hidden`, remove `chat-visible`
- When chat opens for first time, trigger the bot's welcome message after 500ms delay

**Step 2: Define mock sales conversation responses**

Array of bot responses (the agent cycles through these regardless of what user types):

```javascript
const botResponses = [
  "Welcome to Matrix Bot! 🤖 I help businesses automate their customer communication through WhatsApp AI agents. What kind of business do you run?",
  "That's great! Our AI agents can handle appointment booking, answer FAQs, and engage your customers 24/7 — all through WhatsApp. Would you like to know how it works?",
  "Here's how it works: We connect an AI-powered agent to your business WhatsApp number. It learns your services, prices, and policies — then handles customer conversations automatically. It even mimics your communication style!",
  "Our agents can: ✅ Book appointments automatically ✅ Answer common questions ✅ Handle cancellations and rescheduling ✅ Send reminders ✅ Speak in your brand's voice. Want to see pricing?",
  "We offer flexible plans starting at $99/month. That includes unlimited conversations, custom voice profiling, and full integration with your booking system. Shall I set up a demo for your business?",
  "Perfect! To get started, I'd just need your business name and WhatsApp number. We can have your AI agent up and running within 24 hours. Ready to transform your customer experience?"
];
```

**Step 3: Implement message sending and bot response logic**

JavaScript functions:
- `addMessage(text, sender)`: Creates a `.message` div with class `bot` or `user`, appends to `#chat-messages`, scrolls to bottom
- `showTypingIndicator()`: Adds a `.typing-indicator` element with flickering "Matrix Bot is typing..." text, using a character scramble effect (rapidly cycling random characters before settling on the actual text)
- `removeTypingIndicator()`: Removes the typing element
- `handleUserMessage()`: Get input value, clear input, call `addMessage(text, 'user')`, show typing indicator, after 1500ms delay remove indicator and call `addMessage(botResponses[responseIndex++], 'bot')`. If responseIndex exceeds array, loop back to a generic "Let me connect you with our team..." message
- Bind to send button click and Enter key on input

**Step 4: Verify in browser**

Click "Talk to our Agent", type messages, verify conversation flow.
Expected: Chat opens with welcome message, user messages appear right-aligned, bot responds after typing delay with sales pitch sequence.

**Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add chat widget interaction logic with mock sales responses"
```

---

### Task 5: Features Section

**Files:**
- Modify: `index.html`

**Step 1: Add features HTML**

Add below hero section, before chat widget:

```html
<section id="features">
  <h2 class="section-title">Why Matrix Bot?</h2>
  <div class="features-grid">
    <div class="feature-card">
      <div class="feature-icon">⚡</div>
      <h3>24/7 AI Agent</h3>
      <p>Never miss a customer. Your AI agent handles inquiries around the clock.</p>
    </div>
    <div class="feature-card">
      <div class="feature-icon">💬</div>
      <h3>WhatsApp Native</h3>
      <p>Meet your customers where they already are. No app downloads needed.</p>
    </div>
    <div class="feature-card">
      <div class="feature-icon">🎙️</div>
      <h3>Voice Cloning</h3>
      <p>Your agent speaks like you. AI-powered voice profiling captures your brand's tone.</p>
    </div>
    <div class="feature-card">
      <div class="feature-icon">📅</div>
      <h3>Smart Booking</h3>
      <p>Automated scheduling, reminders, and cancellations. Zero manual work.</p>
    </div>
  </div>
</section>
```

**Step 2: Style the features section**

CSS:
- `#features`: padding 100px 40px, max-width 1100px, margin 0 auto
- `.section-title`: color #0F0, text-align center, font-size 2rem, margin-bottom 60px, text-shadow 0 0 10px rgba(0,255,0,0.5)
- `.features-grid`: display grid, grid-template-columns repeat(auto-fit, minmax(240px, 1fr)), gap 30px
- `.feature-card`: background rgba(0,0,0,0.7), border 1px solid rgba(0,255,0,0.2), border-radius 8px, padding 30px 25px, text-align center, transition all 0.3s. On hover: border-color #0F0, box-shadow 0 0 20px rgba(0,255,0,0.15), transform translateY(-5px)
- `.feature-icon`: font-size 2.5rem, margin-bottom 15px
- `.feature-card h3`: color #0F0, font-size 1.2rem, margin-bottom 10px
- `.feature-card p`: color rgba(0,255,0,0.7), font-size 0.9rem, line-height 1.5

**Step 3: Verify in browser**

Scroll below the fold.
Expected: 4 feature cards in a grid, dark backgrounds with green borders, hover glow effect.

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add features section with Matrix-themed cards"
```

---

### Task 6: Footer + Mobile Responsiveness + Polish

**Files:**
- Modify: `index.html`

**Step 1: Add footer HTML**

```html
<footer id="footer">
  <p>&copy; 2026 Matrix Bot. All rights reserved.</p>
</footer>
```

**Step 2: Style footer**

- `#footer`: text-align center, padding 40px 20px, border-top 1px solid rgba(0,255,0,0.1), color rgba(0,255,0,0.5), font-size 0.8rem

**Step 3: Add mobile responsive CSS**

Media query `@media (max-width: 768px)`:
- `.logo`: font-size 2.5rem
- `.tagline`: font-size 1rem, padding 0 20px
- `#chat-widget`: width calc(100% - 20px), right 10px, bottom 10px, height 70vh
- `.features-grid`: grid-template-columns 1fr, padding 0 20px

Media query `@media (max-width: 480px)`:
- `.logo`: font-size 2rem
- `#chat-widget`: height 80vh

**Step 4: Add final polish**

- CSS `@keyframes fadeIn` for message appearance
- CSS `@keyframes glow-pulse` for logo (alternate text-shadow intensity)
- CSS `@keyframes pulse-dot` for chat status dot (scale 1 to 1.3)
- CSS `@keyframes flicker` for typing indicator (opacity 0.4 to 1)
- Smooth scroll behavior: `html { scroll-behavior: smooth }`
- Selection color: `::selection { background: rgba(0,255,0,0.3); color: #0F0 }`

**Step 5: Verify in browser**

Test at various window sizes. Test the full flow: land on page → see rain → see hero → click CTA → chat opens → send messages → scroll to features.
Expected: Fully functional, responsive Matrix-themed landing page.

**Step 6: Commit**

```bash
git add index.html
git commit -m "feat: add footer, mobile responsiveness, and animation polish"
```

---

## Verification Checklist

- [ ] Matrix rain animates smoothly on full screen
- [ ] Rain resizes correctly on window resize
- [ ] Hero text glows and pulses
- [ ] CTA button opens chat widget
- [ ] Chat widget sends/receives messages
- [ ] Typing indicator shows character flicker
- [ ] Features cards have hover effects
- [ ] Page is responsive on mobile widths
- [ ] All content readable over the rain background
