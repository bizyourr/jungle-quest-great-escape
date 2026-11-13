<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>JUNGLE QUEST: THE GREAT ESCAPE!</title>
  <!-- Load Google Fonts for friendly, bold child-appropriate typography -->
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@500;600;700&family=Nunito:wght@600;700;800;900&display=swap" rel="stylesheet" />

  <style>
    /* ==========================================================================
       1. CSS VARIABLES & RESET
       ========================================================================== */
    :root {
      --jungle-dark: #0e3b24;
      --jungle-mid: #1a5c38;
      --jungle-light: #38a169;
      --leaf-bright: #68d391;
      --gold-primary: #fbbf24;
      --gold-dark: #d97706;
      --sky-top: #7dd3fc;
      --sky-bottom: #fef08a;
      --wood-light: #c07a3e;
      --wood-dark: #783f12;
      --cream-card: #fffbeb;
      --text-dark: #1e293b;
      --danger: #ef4444;
      --success: #16a34a;
      --accent-blue: #0284c7;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      user-select: none;
      -webkit-tap-highlight-color: transparent;
    }

    body {
      font-family: 'Nunito', sans-serif;
      background: linear-gradient(180deg, #0b3320 0%, #154c30 50%, #0b3320 100%);
      color: var(--text-dark);
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      overflow-x: hidden;
      padding: 10px;
    }

    h1, h2, h3, .game-font {
      font-family: 'Fredoka', sans-serif;
      letter-spacing: 0.5px;
    }

    /* ==========================================================================
       2. MAIN GAME CONTAINER (RESPONSIVE & GOOGLE SITES FRIENDLY)
       ========================================================================== */
    #game-wrapper {
      width: 100%;
      max-width: 1020px;
      min-height: 640px;
      background: linear-gradient(180deg, #bae6fd 0%, #dcfce7 55%, #86efac 100%);
      border-radius: 28px;
      border: 6px solid #783f12;
      box-shadow: 0 20px 50px rgba(0, 0, 0, 0.55), inset 0 0 0 4px #a16207;
      position: relative;
      overflow: hidden;
      display: flex;
      flex-direction: column;
    }

    /* Decorative Jungle Vines along Top Border */
    .top-vines {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 44px;
      pointer-events: none;
      z-index: 20;
    }

    /* ==========================================================================
       3. TOP HUD (PROGRESS TRACKER, STARS, COINS, SOUND TOGGLE)
       ========================================================================== */
    #hud-bar {
      display: none; /* Shown once game starts */
      align-items: center;
      justify-content: space-between;
      flex-wrap: wrap;
      gap: 10px;
      background: linear-gradient(90deg, #14532d, #166534, #14532d);
      color: #fff;
      padding: 12px 20px;
      border-bottom: 4px solid #783f12;
      z-index: 25;
      position: relative;
    }

    .hud-group {
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .hud-badge {
      background: rgba(0, 0, 0, 0.35);
      border: 2px solid rgba(255, 255, 255, 0.25);
      border-radius: 999px;
      padding: 6px 14px;
      display: flex;
      align-items: center;
      gap: 6px;
      font-weight: 800;
      font-size: 1rem;
      box-shadow: inset 0 2px 4px rgba(0,0,0,0.3);
    }

    .progress-wrapper {
      flex: 1;
      min-width: 180px;
      max-width: 340px;
      display: flex;
      flex-direction: column;
      gap: 4px;
    }

    .progress-label {
      font-size: 0.8rem;
      font-weight: 800;
      text-transform: uppercase;
      color: #fde047;
      display: flex;
      justify-content: space-between;
    }

    .progress-track {
      width: 100%;
      height: 16px;
      background: #052e16;
      border-radius: 999px;
      border: 2px solid #fbbf24;
      overflow: hidden;
      position: relative;
    }

    .progress-fill {
      height: 100%;
      width: 0%;
      background: linear-gradient(90deg, #f59e0b, #fde047, #4ade80);
      border-radius: 999px;
      transition: width 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
    }

    .icon-btn {
      background: #fbbf24;
      color: #451a03;
      border: 3px solid #783f12;
      border-radius: 14px;
      padding: 6px 12px;
      font-family: 'Fredoka', sans-serif;
      font-weight: 700;
      font-size: 0.9rem;
      cursor: pointer;
      box-shadow: 0 3px 0 #783f12;
      transition: transform 0.1s;
    }

    .icon-btn:active {
      transform: translateY(3px);
      box-shadow: 0 0 0 #783f12;
    }

    .icon-btn.mic-ok {
      background: #4ade80;
      color: #052e16;
      border-color: #14532d;
      box-shadow: 0 3px 0 #14532d;
    }

    /* ==========================================================================
       4. SCREENS MANAGEMENT
       ========================================================================== */
    .screen {
      display: none;
      flex: 1;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 24px 20px;
      position: relative;
      z-index: 10;
      animation: fadeSlideIn 0.45s ease-out;
    }

    .screen.active {
      display: flex;
    }

    @keyframes fadeSlideIn {
      from { opacity: 0; transform: translateY(14px) scale(0.98); }
      to { opacity: 1; transform: translateY(0) scale(1); }
    }

    /* ==========================================================================
       5. 3D CARTOON BUTTONS
       ========================================================================== */
    .btn-adventure {
      font-family: 'Fredoka', sans-serif;
      font-size: 1.35rem;
      font-weight: 700;
      color: #451a03;
      background: linear-gradient(180deg, #fde047 0%, #f59e0b 100%);
      border: 4px solid #783f12;
      border-radius: 20px;
      padding: 14px 34px;
      cursor: pointer;
      box-shadow: 0 7px 0 #783f12, 0 12px 20px rgba(0, 0, 0, 0.25);
      transition: all 0.12s ease;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 10px;
      text-transform: uppercase;
    }

    .btn-adventure:hover {
      transform: translateY(-2px);
      filter: brightness(1.06);
    }

    .btn-adventure:active {
      transform: translateY(5px);
      box-shadow: 0 2px 0 #783f12;
    }

    .btn-blue {
      background: linear-gradient(180deg, #38bdf8 0%, #0284c7 100%);
      color: #ffffff;
      border-color: #0c4a6e;
      box-shadow: 0 6px 0 #0c4a6e, 0 10px 18px rgba(0, 0, 0, 0.2);
    }

    .btn-blue:active {
      box-shadow: 0 2px 0 #0c4a6e;
    }

    .btn-green {
      background: linear-gradient(180deg, #4ade80 0%, #16a34a 100%);
      color: #ffffff;
      border-color: #14532d;
      box-shadow: 0 6px 0 #14532d, 0 10px 18px rgba(0, 0, 0, 0.2);
    }

    .btn-green:active {
      box-shadow: 0 2px 0 #14532d;
    }

    .btn-retry {
      background: linear-gradient(180deg, #fb923c 0%, #ea580c 100%);
      color: #ffffff;
      border-color: #7c2d12;
      box-shadow: 0 6px 0 #7c2d12;
      font-size: 1.05rem;
      padding: 10px 22px;
    }

    /* ==========================================================================
       6. WELCOME SCREEN & INSTRUCTION SCREEN
       ========================================================================== */
    .title-banner {
      background: linear-gradient(180deg, #92400e 0%, #783f12 100%);
      border: 5px solid #fbbf24;
      border-radius: 26px;
      padding: 16px 32px;
      text-align: center;
      box-shadow: 0 10px 0 #451a03, 0 15px 30px rgba(0,0,0,0.35);
      margin-bottom: 18px;
      position: relative;
    }

    .title-banner h1 {
      font-size: clamp(1.8rem, 4.5vw, 3rem);
      color: #fde047;
      text-shadow: 3px 3px 0 #451a03, -1px -1px 0 #451a03;
      line-height: 1.1;
    }

    .title-subtitle {
      color: #fef3c7;
      font-weight: 800;
      font-size: clamp(0.95rem, 2vw, 1.15rem);
      margin-top: 6px;
    }

    .story-card {
      background: rgba(255, 251, 235, 0.96);
      border: 4px solid #783f12;
      border-radius: 24px;
      padding: 20px 26px;
      max-width: 720px;
      text-align: center;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
      margin-bottom: 20px;
    }

    .story-text {
      font-size: clamp(1.05rem, 2.2vw, 1.25rem);
      font-weight: 800;
      color: #1e293b;
      line-height: 1.55;
    }

    .welcome-scene-art {
      width: 100%;
      max-width: 680px;
      height: 175px;
      margin-bottom: 14px;
    }

    /* Instructions Grid */
    .steps-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(210px, 1fr));
      gap: 16px;
      width: 100%;
      max-width: 780px;
      margin: 18px 0 24px;
    }

    .step-card {
      background: #ffffff;
      border: 4px solid #166534;
      border-radius: 22px;
      padding: 18px 14px;
      text-align: center;
      box-shadow: 0 6px 0 #14532d;
    }

    .step-num {
      width: 38px;
      height: 38px;
      background: #fbbf24;
      border: 3px solid #783f12;
      border-radius: 50%;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      font-family: 'Fredoka', sans-serif;
      font-weight: 700;
      font-size: 1.2rem;
      color: #451a03;
      margin-bottom: 8px;
    }

    .step-card h3 {
      color: #14532d;
      font-size: 1.2rem;
      margin-bottom: 6px;
    }

    .step-card p {
      font-size: 0.96rem;
      font-weight: 700;
      color: #334155;
    }

    /* Interactive Mic Check Widget */
    .mic-setup-box {
      width: 100%;
      max-width: 780px;
      background: #fffbeb;
      border: 4px solid #f59e0b;
      border-radius: 22px;
      padding: 16px 22px;
      margin-bottom: 18px;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 10px;
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
    }

    .mic-meter-container {
      width: 100%;
      max-width: 420px;
      height: 22px;
      background: #e2e8f0;
      border: 3px solid #475569;
      border-radius: 999px;
      overflow: hidden;
      position: relative;
    }

    .mic-meter-fill {
      height: 100%;
      width: 0%;
      background: linear-gradient(90deg, #22c55e 0%, #eab308 75%, #ef4444 100%);
      border-radius: 999px;
      transition: width 0.08s ease-out;
    }

    .iframe-warning-banner {
      display: none;
      width: 100%;
      background: #fee2e2;
      color: #991b1b;
      border-bottom: 3px solid #ef4444;
      padding: 10px 16px;
      text-align: center;
      font-weight: 800;
      font-size: 0.95rem;
      z-index: 40;
    }

    .iframe-warning-banner a {
      display: inline-block;
      margin-left: 8px;
      background: #dc2626;
      color: #ffffff;
      padding: 4px 12px;
      border-radius: 999px;
      text-decoration: none;
      font-family: 'Fredoka', sans-serif;
    }

    /* ==========================================================================
       7. JUNGLE ADVENTURE MAP SCREEN
       ========================================================================== */
    .map-header {
      text-align: center;
      margin-bottom: 10px;
    }

    .map-header h2 {
      font-size: clamp(1.5rem, 3.5vw, 2.2rem);
      color: #14532d;
      text-shadow: 2px 2px 0 #ffffff;
    }

    .map-header p {
      font-weight: 800;
      color: #166534;
      font-size: 1.05rem;
    }

    #jungle-map-board {
      width: 100%;
      max-width: 880px;
      height: 380px;
      background: linear-gradient(180deg, #86efac 0%, #4ade80 45%, #22c55e 100%);
      border: 5px solid #783f12;
      border-radius: 26px;
      position: relative;
      overflow: hidden;
      box-shadow: inset 0 0 30px rgba(20, 83, 45, 0.35), 0 10px 20px rgba(0, 0, 0, 0.25);
      margin-bottom: 16px;
    }

    .map-node {
      position: absolute;
      transform: translate(-50%, -50%);
      display: flex;
      flex-direction: column;
      align-items: center;
      cursor: pointer;
      z-index: 15;
      transition: transform 0.2s;
    }

    .map-node:hover:not(.locked) {
      transform: translate(-50%, -54%) scale(1.07);
    }

    .node-circle {
      width: 62px;
      height: 62px;
      border-radius: 50%;
      border: 4px solid #783f12;
      display: flex;
      align-items: center;
      justify-content: center;
      font-family: 'Fredoka', sans-serif;
      font-size: 1.5rem;
      font-weight: 700;
      box-shadow: 0 5px 0 #451a03;
      position: relative;
    }

    .map-node.completed .node-circle {
      background: linear-gradient(180deg, #4ade80, #16a34a);
      color: #fff;
    }

    .map-node.active .node-circle {
      background: linear-gradient(180deg, #fde047, #f59e0b);
      color: #451a03;
      animation: pulseNode 1.4s infinite;
    }

    .map-node.locked .node-circle {
      background: #94a3b8;
      color: #334155;
      border-color: #475569;
      box-shadow: 0 4px 0 #334155;
      cursor: not-allowed;
    }

    @keyframes pulseNode {
      0%, 100% { box-shadow: 0 5px 0 #451a03, 0 0 0 0 rgba(251, 191, 36, 0.8); }
      50% { box-shadow: 0 5px 0 #451a03, 0 0 0 14px rgba(251, 191, 36, 0); }
    }

    .node-label {
      margin-top: 6px;
      background: rgba(255, 255, 255, 0.95);
      border: 2px solid #783f12;
      border-radius: 12px;
      padding: 3px 10px;
      font-size: 0.78rem;
      font-weight: 900;
      color: #1e293b;
      white-space: nowrap;
      box-shadow: 0 2px 4px rgba(0,0,0,0.2);
    }

    #map-explorer-avatar {
      position: absolute;
      width: 64px;
      height: 74px;
      transform: translate(-50%, -85%);
      z-index: 18;
      transition: left 0.9s cubic-bezier(0.34, 1.56, 0.64, 1), top 0.9s cubic-bezier(0.34, 1.56, 0.64, 1);
      pointer-events: none;
      filter: drop-shadow(0 5px 4px rgba(0,0,0,0.35));
    }

    /* ==========================================================================
       8. CHECKPOINT STAGE SCREEN (ANIMATED SCENE + PRONUNCIATION CARD)
       ========================================================================== */
    #checkpoint-screen {
      justify-content: flex-start;
      padding-top: 14px;
      gap: 14px;
    }

    .stage-topbar {
      width: 100%;
      max-width: 880px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      gap: 8px;
    }

    .stage-title-pill {
      background: #14532d;
      color: #fde047;
      border: 3px solid #fbbf24;
      border-radius: 999px;
      padding: 6px 18px;
      font-family: 'Fredoka', sans-serif;
      font-size: 1.15rem;
      box-shadow: 0 4px 0 #052e16;
    }

    .question-dots {
      display: flex;
      gap: 8px;
      align-items: center;
      background: rgba(255,255,255,0.85);
      padding: 6px 14px;
      border-radius: 999px;
      border: 2px solid #783f12;
    }

    .q-dot {
      width: 18px;
      height: 18px;
      border-radius: 50%;
      background: #cbd5e1;
      border: 2px solid #64748b;
      transition: all 0.3s;
    }

    .q-dot.current {
      background: #fbbf24;
      border-color: #783f12;
      transform: scale(1.25);
    }

    .q-dot.done {
      background: #22c55e;
      border-color: #14532d;
    }

    /* Interactive Animated Stage Canvas */
    #stage-visual-box {
      width: 100%;
      max-width: 880px;
      height: 185px;
      border-radius: 22px;
      border: 4px solid #783f12;
      overflow: hidden;
      position: relative;
      box-shadow: 0 8px 18px rgba(0, 0, 0, 0.22);
      background: #86efac;
    }

    /* Challenge Card */
    .challenge-card {
      width: 100%;
      max-width: 880px;
      background: #fffbeb;
      border: 4px solid #783f12;
      border-radius: 24px;
      padding: 18px 22px;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
      display: flex;
      flex-direction: column;
      align-items: center;
      text-align: center;
      gap: 12px;
    }

    .instruction-badge {
      background: #e0f2fe;
      color: #0369a1;
      border: 2px solid #38bdf8;
      border-radius: 999px;
      padding: 4px 16px;
      font-weight: 800;
      font-size: 0.92rem;
    }

    .prompt-sentence {
      font-family: 'Fredoka', sans-serif;
      font-size: clamp(1.35rem, 3.2vw, 2rem);
      color: #1e293b;
      background: #fef3c7;
      border: 3px dashed #f59e0b;
      border-radius: 18px;
      padding: 10px 22px;
      width: 100%;
      max-width: 720px;
    }

    .blank-highlight {
      color: #d97706;
      text-decoration: underline;
      text-decoration-style: wavy;
    }

    .expected-box {
      display: flex;
      align-items: center;
      justify-content: center;
      flex-wrap: wrap;
      gap: 8px;
      background: #dcfce7;
      border: 3px solid #22c55e;
      border-radius: 16px;
      padding: 8px 20px;
      font-size: 1.15rem;
      font-weight: 800;
      color: #14532d;
    }

    .expected-word {
      font-family: 'Fredoka', sans-serif;
      font-size: 1.45rem;
      color: #15803d;
      background: #ffffff;
      padding: 2px 14px;
      border-radius: 10px;
      border: 2px solid #86efac;
    }

    /* Controls Row: LISTEN + BIG MIC + RETRY */
    .controls-row {
      display: flex;
      align-items: center;
      justify-content: center;
      flex-wrap: wrap;
      gap: 16px;
      margin-top: 4px;
    }

    .mic-btn {
      font-family: 'Fredoka', sans-serif;
      font-size: 1.25rem;
      font-weight: 700;
      color: #ffffff;
      background: linear-gradient(180deg, #ef4444 0%, #dc2626 100%);
      border: 4px solid #7f1d1d;
      border-radius: 999px;
      padding: 14px 30px;
      cursor: pointer;
      display: inline-flex;
      align-items: center;
      gap: 10px;
      box-shadow: 0 7px 0 #7f1d1d, 0 12px 20px rgba(220, 38, 38, 0.35);
      transition: all 0.15s ease;
    }

    .mic-btn:hover {
      transform: translateY(-2px);
      filter: brightness(1.06);
    }

    .mic-btn:active {
      transform: translateY(4px);
      box-shadow: 0 2px 0 #7f1d1d;
    }

    .mic-btn.listening {
      background: linear-gradient(180deg, #22c55e 0%, #15803d 100%);
      border-color: #052e16;
      box-shadow: 0 5px 0 #052e16, 0 0 0 0 rgba(34, 197, 94, 0.7);
      animation: micPulse 1.1s infinite;
    }

    @keyframes micPulse {
      0% { box-shadow: 0 5px 0 #052e16, 0 0 0 0 rgba(34, 197, 94, 0.75); }
      70% { box-shadow: 0 5px 0 #052e16, 0 0 0 18px rgba(34, 197, 94, 0); }
      100% { box-shadow: 0 5px 0 #052e16, 0 0 0 0 rgba(34, 197, 94, 0); }
    }

    /* Live Stage Mic Volume Bar */
    .stage-mic-meter {
      width: 100%;
      max-width: 340px;
      height: 12px;
      background: #e2e8f0;
      border: 2px solid #64748b;
      border-radius: 999px;
      overflow: hidden;
      display: none;
    }

    .stage-mic-meter.active {
      display: block;
    }

    .stage-mic-meter-fill {
      height: 100%;
      width: 0%;
      background: linear-gradient(90deg, #22c55e, #facc15);
      transition: width 0.07s linear;
    }

    /* Speech Recognition Output & Feedback Area */
    .speech-status-panel {
      width: 100%;
      max-width: 720px;
      min-height: 68px;
      background: #f8fafc;
      border: 3px solid #cbd5e1;
      border-radius: 18px;
      padding: 10px 16px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 6px;
      transition: all 0.3s;
    }

    .speech-status-panel.success {
      background: #dcfce7;
      border-color: #16a34a;
    }

    .speech-status-panel.error {
      background: #fee2e2;
      border-color: #ef4444;
    }

    .speech-status-panel.warning {
      background: #fef3c7;
      border-color: #f59e0b;
    }

    .recognized-text {
      font-size: 1.05rem;
      font-weight: 800;
      color: #334155;
    }

    .feedback-msg {
      font-family: 'Fredoka', sans-serif;
      font-size: 1.2rem;
      font-weight: 700;
    }

    .feedback-msg.success { color: #15803d; }
    .feedback-msg.error { color: #b91c1c; }
    .feedback-msg.warning { color: #b45309; }

    /* ==========================================================================
       9. STAGE ANIMATION KEYFRAMES (GATES, CROCODILE, WAVES, STARS)
       ========================================================================== */
    @keyframes floatBob {
      0%, 100% { transform: translateY(0px); }
      50% { transform: translateY(-6px); }
    }

    @keyframes swayTree {
      0%, 100% { transform: rotate(-1.5deg); }
      50% { transform: rotate(1.5deg); }
    }

    @keyframes echoRing {
      0% { transform: scale(0.6); opacity: 0.9; }
      100% { transform: scale(1.8); opacity: 0; }
    }

    @keyframes starPop {
      0% { transform: scale(0) rotate(-30deg); opacity: 0; }
      70% { transform: scale(1.25) rotate(10deg); opacity: 1; }
      100% { transform: scale(1) rotate(0deg); opacity: 1; }
    }

    .star-pop-anim {
      animation: starPop 0.55s cubic-bezier(0.34, 1.56, 0.64, 1) forwards;
    }

    /* ==========================================================================
       10. VICTORY SCREEN & CONFETTI
       ========================================================================== */
    #confetti-canvas {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
      z-index: 30;
    }

    .victory-banner {
      background: linear-gradient(180deg, #fde047 0%, #f59e0b 100%);
      border: 5px solid #783f12;
      border-radius: 26px;
      padding: 18px 28px;
      text-align: center;
      box-shadow: 0 10px 0 #783f12;
      margin-bottom: 16px;
    }

    .victory-banner h1 {
      font-size: clamp(1.7rem, 4vw, 2.7rem);
      color: #451a03;
      text-shadow: 2px 2px 0 #fef08a;
    }

    .score-summary-box {
      background: #ffffff;
      border: 4px solid #166534;
      border-radius: 24px;
      padding: 18px 28px;
      display: flex;
      gap: 24px;
      flex-wrap: wrap;
      justify-content: center;
      box-shadow: 0 8px 0 #14532d;
      margin-bottom: 18px;
    }

    .score-stat {
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    .score-stat-val {
      font-family: 'Fredoka', sans-serif;
      font-size: 2.2rem;
      font-weight: 700;
      color: #d97706;
    }

    .score-stat-label {
      font-weight: 800;
      color: #14532d;
      font-size: 0.95rem;
    }

    /* ==========================================================================
       11. RESPONSIVE ADJUSTMENTS
       ========================================================================== */
    @media (max-width: 640px) {
      #game-wrapper {
        border-radius: 18px;
        border-width: 4px;
        min-height: 580px;
      }
      .screen {
        padding: 16px 12px;
      }
      #jungle-map-board {
        height: 330px;
      }
      .node-circle {
        width: 50px;
        height: 50px;
        font-size: 1.25rem;
      }
      .node-label {
        font-size: 0.7rem;
        padding: 2px 7px;
      }
      #stage-visual-box {
        height: 150px;
      }
      .mic-btn {
        font-size: 1.1rem;
        padding: 12px 22px;
      }
      .btn-adventure {
        font-size: 1.15rem;
        padding: 12px 24px;
      }
    }
  </style>
</head>
<body>

  <div id="game-wrapper">
    <!-- Iframe / Google Sites Microphone Permission Warning Banner -->
    <div id="iframe-mic-banner" class="iframe-warning-banner">
      ⚠️ Embedded Window Detected: If your microphone is blocked by Google Sites, open the game in a full tab!
      <a id="btn-open-full-tab" href="#" target="_blank" rel="noopener noreferrer">↗️ Open Full Tab</a>
    </div>

    <!-- Decorative Hanging Vines SVG at Top -->
    <svg class="top-vines" viewBox="0 0 1000 45" preserveAspectRatio="none">
      <path d="M0,0 Q80,42 160,0 Q240,35 320,0 Q420,44 520,0 Q620,36 720,0 Q850,42 1000,0 L1000,0 L0,0 Z" fill="#15803d" opacity="0.85"/>
      <ellipse cx="90" cy="22" rx="14" ry="7" fill="#4ade80" transform="rotate(20 90 22)"/>
      <ellipse cx="260" cy="19" rx="14" ry="7" fill="#22c55e" transform="rotate(-15 260 19)"/>
      <ellipse cx="440" cy="24" rx="15" ry="7" fill="#4ade80" transform="rotate(12 440 24)"/>
      <ellipse cx="640" cy="18" rx="14" ry="7" fill="#22c55e" transform="rotate(-20 640 18)"/>
      <ellipse cx="860" cy="22" rx="15" ry="7" fill="#4ade80" transform="rotate(18 860 22)"/>
    </svg>

    <!-- ======================================================================
         TOP HUD BAR (Visible during Map & Checkpoints)
         ====================================================================== -->
    <header id="hud-bar">
      <div class="hud-group">
        <button class="icon-btn" id="btn-to-map" title="Return to Jungle Map">🗺️ Map</button>
        <div class="hud-badge" title="Stars Earned">
          <span>⭐</span>
          <span id="hud-stars">0</span> / <span id="hud-total-stars">18</span>
        </div>
        <div class="hud-badge" title="Jungle Coins">
          <span>🪙</span>
          <span id="hud-coins">0</span>
        </div>
      </div>

      <div class="progress-wrapper">
        <div class="progress-label">
          <span>Jungle Escape Progress</span>
          <span id="hud-progress-pct">0%</span>
        </div>
        <div class="progress-track">
          <div class="progress-fill" id="hud-progress-fill"></div>
        </div>
      </div>

      <div class="hud-group">
        <button class="icon-btn" id="btn-hud-mic-check" title="Test or Enable Microphone">🎤 Mic Setup</button>
        <button class="icon-btn" id="btn-sound-toggle" title="Toggle Sound Effects">🔊 SFX On</button>
      </div>
    </header>

    <!-- ======================================================================
         SCREEN 1: WELCOME SCREEN
         ====================================================================== -->
    <section id="welcome-screen" class="screen active">
      <div class="title-banner">
        <h1>🌴 JUNGLE QUEST: THE GREAT ESCAPE! 🌴</h1>
        <div class="title-subtitle">An Interactive English Speaking Adventure for Explorers</div>
      </div>

      <!-- Cartoon Jungle & Explorer Illustration -->
      <svg class="welcome-scene-art" viewBox="0 0 680 175" xmlns="http://www.w3.org/2000/svg">
        <!-- Distant Hills & Waterfall -->
        <ellipse cx="140" cy="165" rx="180" ry="70" fill="#22c55e"/>
        <ellipse cx="530" cy="165" rx="200" ry="75" fill="#16a34a"/>
        <!-- Waterfall Cliff -->
        <rect x="475" y="45" width="90" height="115" rx="14" fill="#64748b"/>
        <rect x="495" y="50" width="46" height="110" fill="#38bdf8"/>
        <path d="M505,50 L505,155 M520,50 L520,155 M533,50 L533,155" stroke="#e0f2fe" stroke-width="3" stroke-dasharray="10 8"/>
        <ellipse cx="518" cy="160" rx="45" ry="12" fill="#bae6fd"/>

        <!-- Palm Trees -->
        <g transform="translate(55, 25)">
          <path d="M22,135 Q12,80 26,35" stroke="#783f12" stroke-width="12" fill="none" stroke-linecap="round"/>
          <path d="M26,35 Q-10,20 -25,45" stroke="#15803d" stroke-width="10" fill="none" stroke-linecap="round"/>
          <path d="M26,35 Q5,5 -15,15" stroke="#22c55e" stroke-width="10" fill="none" stroke-linecap="round"/>
          <path d="M26,35 Q60,15 75,40" stroke="#15803d" stroke-width="10" fill="none" stroke-linecap="round"/>
          <path d="M26,35 Q50,2 70,12" stroke="#22c55e" stroke-width="10" fill="none" stroke-linecap="round"/>
        </g>

        <!-- Cute Monkey Peeking -->
        <g transform="translate(150, 75)">
          <circle cx="25" cy="28" r="18" fill="#78350f"/>
          <circle cx="8" cy="26" r="7" fill="#fcd34d"/>
          <circle cx="42" cy="26" r="7" fill="#fcd34d"/>
          <ellipse cx="25" cy="31" rx="13" ry="11" fill="#fde68a"/>
          <circle cx="20" cy="26" r="2.5" fill="#1e293b"/>
          <circle cx="30" cy="26" r="2.5" fill="#1e293b"/>
          <path d="M20,34 Q25,39 30,34" stroke="#78350f" stroke-width="2.5" fill="none" stroke-linecap="round"/>
        </g>

        <!-- Hero Cartoon Explorer Character with Backpack -->
        <g transform="translate(295, 22)" style="animation: floatBob 2.4s ease-in-out infinite;">
          <!-- Backpack -->
          <rect x="12" y="64" width="56" height="52" rx="14" fill="#d97706" stroke="#451a03" stroke-width="3"/>
          <rect x="18" y="72" width="44" height="16" rx="6" fill="#fbbf24" stroke="#451a03" stroke-width="2"/>
          <!-- Legs & Boots -->
          <rect x="26" y="112" width="11" height="24" rx="4" fill="#1e3a8a"/>
          <rect x="43" y="112" width="11" height="24" rx="4" fill="#1e3a8a"/>
          <ellipse cx="30" cy="137" rx="9" ry="5" fill="#783f12"/>
          <ellipse cx="49" cy="137" rx="9" ry="5" fill="#783f12"/>
          <!-- Torso (Explorer Vest) -->
          <rect x="22" y="66" width="36" height="48" rx="12" fill="#fef3c7" stroke="#783f12" stroke-width="3"/>
          <rect x="22" y="66" width="13" height="48" rx="5" fill="#65a30d" stroke="#365314" stroke-width="2"/>
          <rect x="45" y="66" width="13" height="48" rx="5" fill="#65a30d" stroke="#365314" stroke-width="2"/>
          <!-- Waving Arms -->
          <path d="M22,74 Q5,55 12,40" stroke="#fed7aa" stroke-width="9" stroke-linecap="round" fill="none"/>
          <path d="M58,74 Q74,88 68,104" stroke="#fed7aa" stroke-width="9" stroke-linecap="round" fill="none"/>
          <!-- Head -->
          <circle cx="40" cy="44" r="23" fill="#fed7aa" stroke="#783f12" stroke-width="3"/>
          <!-- Eyes & Happy Smile -->
          <circle cx="33" cy="43" r="3.2" fill="#1e293b"/>
          <circle cx="47" cy="43" r="3.2" fill="#1e293b"/>
          <circle cx="34" cy="42" r="1" fill="#ffffff"/>
          <circle cx="48" cy="42" r="1" fill="#ffffff"/>
          <circle cx="27" cy="49" r="3.5" fill="#fca5a5" opacity="0.7"/>
          <circle cx="53" cy="49" r="3.5" fill="#fca5a5" opacity="0.7"/>
          <path d="M33,52 Q40,60 47,52" stroke="#783f12" stroke-width="3" fill="none" stroke-linecap="round"/>
          <!-- Safari Explorer Hat -->
          <ellipse cx="40" cy="24" rx="34" ry="8" fill="#d97706" stroke="#451a03" stroke-width="3"/>
          <path d="M20,24 Q20,6 40,6 Q60,6 60,24 Z" fill="#f59e0b" stroke="#451a03" stroke-width="3"/>
          <rect x="21" y="19" width="38" height="5" fill="#783f12"/>
        </g>

        <!-- Colorful Parrot on Right -->
        <g transform="translate(575, 55)">
          <ellipse cx="24" cy="36" rx="14" ry="22" fill="#ef4444" stroke="#7f1d1d" stroke-width="2.5"/>
          <path d="M16,32 Q8,45 18,54" fill="#3b82f6"/>
          <circle cx="24" cy="18" r="11" fill="#ef4444" stroke="#7f1d1d" stroke-width="2.5"/>
          <circle cx="28" cy="16" r="4" fill="#ffffff"/>
          <circle cx="29" cy="16" r="2" fill="#000000"/>
          <path d="M34,17 Q43,19 35,25 Z" fill="#fbbf24" stroke="#783f12" stroke-width="1.5"/>
        </g>

        <!-- Jungle Grass Foreground -->
        <rect x="0" y="155" width="680" height="25" fill="#15803d"/>
      </svg>

      <!-- Exact Required Story Introduction -->
      <div class="story-card">
        <p class="story-text">
          “OH NO! You are lost in a mysterious jungle! The way home is locked behind five challenging checkpoints. Can you use your English speaking skills to overcome every obstacle and escape? Let the adventure begin!”
        </p>
      </div>

      <button class="btn-adventure" id="btn-start-journey">
        🧭 START JOURNEY
      </button>
    </section>

    <!-- ======================================================================
         SCREEN 2: SHORT INSTRUCTION SCREEN
         ====================================================================== -->
    <section id="instruction-screen" class="screen">
      <div class="title-banner" style="padding: 12px 26px;">
        <h1 style="font-size: clamp(1.5rem, 3.5vw, 2.3rem);">🎒 HOW TO PLAY, EXPLORER!</h1>
      </div>

      <div class="steps-grid">
        <div class="step-card">
          <div class="step-num">1</div>
          <div style="font-size: 2.3rem; margin-bottom: 4px;">👀</div>
          <h3>Read the Clue</h3>
          <p>Look at the jungle challenge sentence and check the missing English word or phrase.</p>
        </div>

        <div class="step-card">
          <div class="step-num">2</div>
          <div style="font-size: 2.3rem; margin-bottom: 4px;">🔊</div>
          <h3>Press LISTEN</h3>
          <p>Tap the blue <strong>LISTEN</strong> button to hear how the English word or sentence is spoken.</p>
        </div>

        <div class="step-card">
          <div class="step-num">3</div>
          <div style="font-size: 2.3rem; margin-bottom: 4px;">🎤</div>
          <h3>Speak Clearly!</h3>
          <p>Tap the red <strong>SPEAK NOW</strong> microphone button and say the answer aloud to unlock the path!</p>
        </div>
      </div>

      <!-- Interactive Device Microphone Permission & Live Volume Test Widget -->
      <div class="mic-setup-box">
        <div style="font-family: 'Fredoka', sans-serif; font-size: 1.2rem; color: #783f12;">
          🎙️ STEP 0: ENABLE & TEST YOUR DEVICE MICROPHONE
        </div>
        <button class="btn-adventure btn-blue" id="btn-test-mic" style="font-size: 1.05rem; padding: 10px 24px;">
          🎤 TEST MY MICROPHONE
        </button>
        <div class="mic-meter-container" title="Live Microphone Volume Level">
          <div class="mic-meter-fill" id="setup-mic-meter-fill"></div>
        </div>
        <div id="mic-setup-status" style="font-weight: 800; color: #334155; font-size: 0.95rem; text-align: center;">
          Click <strong>TEST MY MICROPHONE</strong> and tap <strong>Allow</strong> when your browser asks!
        </div>
      </div>

      <button class="btn-adventure btn-green" id="btn-to-jungle-map">
        🗺️ ENTER THE JUNGLE MAP
      </button>
    </section>

    <!-- ======================================================================
         SCREEN 3: INTERACTIVE JUNGLE ADVENTURE MAP
         ====================================================================== -->
    <section id="map-screen" class="screen">
      <div class="map-header">
        <h2>🗺️ JUNGLE EXPEDITION MAP</h2>
        <p id="map-status-text">Click on the glowing Checkpoint to continue your escape!</p>
      </div>

      <div id="jungle-map-board">
        <!-- Winding Jungle River, Trees, Pathway & Exit Temple SVG -->
        <svg viewBox="0 0 880 380" preserveAspectRatio="none" style="width:100%; height:100%; position:absolute; top:0; left:0;">
          <!-- River flowing through Checkpoint 2 -->
          <path d="M260,0 C230,120 330,240 280,380 L355,380 C405,240 305,120 335,0 Z" fill="#38bdf8"/>
          <path d="M295,10 C275,120 365,240 315,370" stroke="#bae6fd" stroke-width="5" stroke-dasharray="14 12" fill="none"/>

          <!-- Winding Dirt Jungle Trail connecting all 5 Checkpoints -->
          <path d="M115,285 Q210,320 310,210 T510,255 T680,145 T805,85"
                stroke="#d97706" stroke-width="26" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
          <path d="M115,285 Q210,320 310,210 T510,255 T680,145 T805,85"
                stroke="#fde68a" stroke-width="6" stroke-dasharray="14 14" fill="none" stroke-linecap="round"/>

          <!-- Decorative Trees & Bushes -->
          <circle cx="65" cy="75" r="42" fill="#15803d"/>
          <circle cx="105" cy="60" r="35" fill="#16a34a"/>
          <circle cx="445" cy="65" r="45" fill="#15803d"/>
          <circle cx="490" cy="80" r="32" fill="#16a34a"/>
          <circle cx="180" cy="145" r="30" fill="#15803d"/>
          <circle cx="615" cy="315" r="46" fill="#15803d"/>
          <circle cx="665" cy="330" r="35" fill="#16a34a"/>
          <circle cx="795" cy="275" r="42" fill="#15803d"/>

          <!-- Exit Sunshine Glow near Checkpoint 5 -->
          <circle cx="805" cy="85" r="55" fill="#fef08a" opacity="0.55"/>
        </svg>

        <!-- 5 Checkpoint Nodes injected dynamically by JS -->
        <div id="map-nodes-container"></div>

        <!-- Animated Explorer Avatar on Map -->
        <div id="map-explorer-avatar">
          <svg viewBox="0 0 80 95" width="64" height="74">
            <rect x="14" y="42" width="52" height="36" rx="12" fill="#d97706" stroke="#451a03" stroke-width="3"/>
            <rect x="24" y="44" width="32" height="36" rx="10" fill="#65a30d" stroke="#365314" stroke-width="3"/>
            <circle cx="40" cy="28" r="19" fill="#fed7aa" stroke="#783f12" stroke-width="3"/>
            <circle cx="34" cy="27" r="2.8" fill="#1e293b"/>
            <circle cx="46" cy="27" r="2.8" fill="#1e293b"/>
            <path d="M34,34 Q40,40 46,34" stroke="#783f12" stroke-width="2.5" fill="none" stroke-linecap="round"/>
            <ellipse cx="40" cy="13" rx="28" ry="6" fill="#d97706" stroke="#451a03" stroke-width="2.5"/>
            <path d="M23,13 Q23,1 40,1 Q57,1 57,13 Z" fill="#f59e0b" stroke="#451a03" stroke-width="2.5"/>
          </svg>
        </div>
      </div>

      <button class="btn-adventure" id="btn-enter-active-checkpoint">
        🚀 ENTER CHECKPOINT 1
      </button>
    </section>

    <!-- ======================================================================
         SCREEN 4: CHECKPOINT CHALLENGE SCREEN
         ====================================================================== -->
    <section id="checkpoint-screen" class="screen">
      <div class="stage-topbar">
        <div class="stage-title-pill" id="stage-header-title">
          Checkpoint 1: The Whispering Jungle
        </div>
        <div class="question-dots" id="question-progress-dots">
          <!-- Dots generated per question -->
        </div>
      </div>

      <!-- Dynamic Visual Scene for the Active Checkpoint -->
      <div id="stage-visual-box">
        <!-- Animated SVG scene rendered dynamically for each checkpoint -->
      </div>

      <!-- Interactive Pronunciation Card -->
      <div class="challenge-card">
        <div class="instruction-badge" id="challenge-instruction">
          Say the missing word aloud to unlock the gate!
        </div>

        <div class="prompt-sentence" id="prompt-display">
          “Good morning, <span class="blank-highlight">______</span>!”
        </div>

        <div class="expected-box">
          <span>🗣️ Say aloud:</span>
          <span class="expected-word" id="expected-answer-display">Teacher</span>
        </div>

        <!-- Primary Interactive Buttons -->
        <div class="controls-row">
          <button class="btn-adventure btn-blue" id="btn-listen" style="font-size: 1.1rem; padding: 12px 22px;">
            🔊 LISTEN
          </button>

          <button class="mic-btn" id="btn-speak">
            <span id="mic-icon">🎤</span>
            <span id="mic-btn-text">SPEAK NOW</span>
          </button>

          <button class="btn-adventure btn-retry" id="btn-retry" style="display: none;">
            🔄 TRY AGAIN
          </button>
        </div>

        <!-- Live Volume Meter during Speaking -->
        <div class="stage-mic-meter" id="stage-mic-meter">
          <div class="stage-mic-meter-fill" id="stage-mic-meter-fill"></div>
        </div>

        <!-- Speech Recognition Feedback Box -->
        <div class="speech-status-panel" id="speech-panel">
          <div class="recognized-text" id="recognized-display">
            Press <strong>LISTEN</strong> to hear it, then press <strong>SPEAK NOW</strong>!
          </div>
          <div class="feedback-msg" id="feedback-display"></div>
        </div>

        <!-- Next Question / Continue Button (Appears after correct answer) -->
        <button class="btn-adventure btn-green" id="btn-next-question" style="display: none;">
          ⭐ CONTINUE JOURNEY ➔
        </button>
      </div>
    </section>

    <!-- ======================================================================
         SCREEN 5: FINAL VICTORY SCREEN
         ====================================================================== -->
    <section id="victory-screen" class="screen">
      <canvas id="confetti-canvas"></canvas>

      <div class="victory-banner">
        <h1>🎉 CONGRATULATIONS! YOU ESCAPED THE JUNGLE! 🎉</h1>
      </div>

      <!-- Celebration Artwork -->
      <svg viewBox="0 0 600 155" style="width: 100%; max-width: 560px; height: 145px; margin-bottom: 12px;">
        <!-- Bright Sunshine Rays -->
        <circle cx="300" cy="75" r="55" fill="#fde047" opacity="0.75"/>
        <circle cx="300" cy="75" r="38" fill="#fbbf24"/>
        <!-- Open Golden Temple Gate -->
        <rect x="195" y="25" width="35" height="115" rx="6" fill="#d97706" stroke="#783f12" stroke-width="3"/>
        <rect x="370" y="25" width="35" height="115" rx="6" fill="#d97706" stroke="#783f12" stroke-width="3"/>
        <rect x="180" y="12" width="240" height="22" rx="8" fill="#f59e0b" stroke="#783f12" stroke-width="3"/>
        <!-- Cheering Explorer Jumping -->
        <g transform="translate(260, 22)" style="animation: floatBob 1.2s ease-in-out infinite;">
          <rect x="22" y="58" width="36" height="44" rx="10" fill="#65a30d" stroke="#365314" stroke-width="3"/>
          <path d="M22,64 Q4,36 14,22" stroke="#fed7aa" stroke-width="9" stroke-linecap="round" fill="none"/>
          <path d="M58,64 Q76,36 66,22" stroke="#fed7aa" stroke-width="9" stroke-linecap="round" fill="none"/>
          <circle cx="40" cy="38" r="21" fill="#fed7aa" stroke="#783f12" stroke-width="3"/>
          <circle cx="33" cy="36" r="3" fill="#1e293b"/>
          <circle cx="47" cy="36" r="3" fill="#1e293b"/>
          <path d="M32,44 Q40,54 48,44 Z" fill="#ef4444" stroke="#783f12" stroke-width="2"/>
          <ellipse cx="40" cy="19" rx="30" ry="7" fill="#d97706" stroke="#451a03" stroke-width="2.5"/>
          <path d="M22,19 Q22,4 40,4 Q58,4 58,19 Z" fill="#f59e0b" stroke="#451a03" stroke-width="2.5"/>
        </g>
        <!-- Ground -->
        <rect x="0" y="135" width="600" height="25" rx="10" fill="#22c55e"/>
      </svg>

      <div class="story-card" style="margin-bottom: 14px; padding: 14px 22px;">
        <p class="story-text" style="font-size: 1.1rem;">
          Amazing work, brave explorer! You conquered every jungle obstacle using your English speaking skills and found your way safely home!
        </p>
      </div>

      <div class="score-summary-box">
        <div class="score-stat">
          <div class="score-stat-val" id="final-stars-text">⭐ 18 / 18</div>
          <div class="score-stat-label">TOTAL STARS EARNED</div>
        </div>
        <div class="score-stat">
          <div class="score-stat-val" id="final-coins-text">🪙 180</div>
          <div class="score-stat-label">JUNGLE COINS</div>
        </div>
        <div class="score-stat">
          <div class="score-stat-val" style="color: #16a34a;">🏆 100%</div>
          <div class="score-stat-label">JUNGLE ESCAPED</div>
        </div>
      </div>

      <button class="btn-adventure" id="btn-play-again">
        🔄 PLAY AGAIN
      </button>
    </section>
  </div>

  <!-- ========================================================================
       JAVASCRIPT: GAME DATA, SPEECH RECOGNITION, TTS, AUDIO & ANIMATIONS
       ======================================================================== -->
  <script>
    /**
     * ========================================================================
     * 1. CURRICULUM & CHECKPOINT DATA (EASY FOR TEACHERS TO CUSTOMIZE)
     * ========================================================================
     * Each checkpoint contains:
     * - id: 1 to 5
     * - name: Title displayed on the map and stage header
     * - mapCoords: { x: %, y: % } position on the Jungle Map
     * - sceneType: 'gate' | 'river' | 'cave' | 'bridge' | 'final_gate'
     * - instruction: Helpful guidance for Year 4 ESL pupils
     * - questions: Array of pronunciation challenges
     *     - promptHTML: Sentence shown on screen
     *     - targetWord: Exact word/phrase displayed for the pupil to speak
     *     - speakModel: What Text-to-Speech reads aloud when LISTEN is clicked
     *     - acceptedAnswers: Normalized lowercase alternatives accepted
     */
    const CHECKPOINTS = [
      {
        id: 1,
        name: "The Whispering Jungle",
        shortName: "1. Whispering Jungle",
        mapCoords: { x: 14, y: 74 },
        sceneType: "gate",
        instruction: "Speak the missing classroom word to unlock the wooden gate!",
        questions: [
          {
            promptHTML: "“Good morning, <span class='blank-highlight'>______</span>!”",
            targetWord: "Teacher",
            speakModel: "Teacher",
            acceptedAnswers: ["teacher", "good morning teacher"]
          },
          {
            promptHTML: "“Please open your <span class='blank-highlight'>______</span>.”",
            targetWord: "Book",
            speakModel: "Book",
            acceptedAnswers: ["book", "books", "please open your book"]
          },
          {
            promptHTML: "“Where is my <span class='blank-highlight'>______</span>?”",
            targetWord: "Pencil",
            speakModel: "Pencil",
            acceptedAnswers: ["pencil", "pencils", "where is my pencil"]
          },
          {
            promptHTML: "“I have a <span class='blank-highlight'>______</span>.”",
            targetWord: "Question",
            speakModel: "Question",
            acceptedAnswers: ["question", "questions", "i have a question"]
          }
        ]
      },
      {
        id: 2,
        name: "The Crocodile River",
        shortName: "2. Crocodile River",
        mapCoords: { x: 35, y: 55 },
        sceneType: "river",
        instruction: "Say the missing classroom word to raise a stepping stone over the crocodile!",
        questions: [
          {
            promptHTML: "“Can you <span class='blank-highlight'>______</span> me, please?”",
            targetWord: "Help",
            speakModel: "Help",
            acceptedAnswers: ["help", "can you help me please", "can you help me"]
          },
          {
            promptHTML: "“May I <span class='blank-highlight'>______</span>, teacher?”",
            targetWord: "Come in",
            speakModel: "Come in",
            acceptedAnswers: ["come in", "may i come in teacher", "may i come in"]
          },
          {
            promptHTML: "“Please <span class='blank-highlight'>______</span> carefully.”",
            targetWord: "Listen",
            speakModel: "Listen",
            acceptedAnswers: ["listen", "please listen carefully", "please listen"]
          }
        ]
      },
      {
        id: 3,
        name: "The Echoing Cave",
        shortName: "3. Echoing Cave",
        mapCoords: { x: 56, y: 66 },
        sceneType: "cave",
        instruction: "Listen to the cave echo, then pronounce the word clearly!",
        autoPlayAudio: true,
        questions: [
          {
            promptHTML: "Echo Word 1: <span class='blank-highlight'>Understand</span>",
            targetWord: "Understand",
            speakModel: "Understand",
            acceptedAnswers: ["understand", "i understand"]
          },
          {
            promptHTML: "Echo Word 2: <span class='blank-highlight'>Repeat</span>",
            targetWord: "Repeat",
            speakModel: "Repeat",
            acceptedAnswers: ["repeat"]
          },
          {
            promptHTML: "Echo Word 3: <span class='blank-highlight'>Question</span>",
            targetWord: "Question",
            speakModel: "Question",
            acceptedAnswers: ["question", "questions"]
          },
          {
            promptHTML: "Echo Word 4: <span class='blank-highlight'>Answer</span>",
            targetWord: "Answer",
            speakModel: "Answer",
            acceptedAnswers: ["answer", "answers"]
          }
        ]
      },
      {
        id: 4,
        name: "The Broken Bridge",
        shortName: "4. Broken Bridge",
        mapCoords: { x: 76, y: 38 },
        sceneType: "bridge",
        instruction: "Pronounce the full classroom expression to repair each wooden plank!",
        questions: [
          {
            promptHTML: "“Could you repeat that, please?”",
            targetWord: "Could you repeat that, please?",
            speakModel: "Could you repeat that, please?",
            acceptedAnswers: ["could you repeat that please", "could you repeat that"]
          },
          {
            promptHTML: "“I do not understand.”",
            targetWord: "I do not understand.",
            speakModel: "I do not understand.",
            acceptedAnswers: ["i do not understand", "i don't understand", "i dont understand"]
          },
          {
            promptHTML: "“Can you help me with this question?”",
            targetWord: "Can you help me with this question?",
            speakModel: "Can you help me with this question?",
            acceptedAnswers: ["can you help me with this question", "can you help me with the question"]
          },
          {
            promptHTML: "“May I borrow your pencil?”",
            targetWord: "May I borrow your pencil?",
            speakModel: "May I borrow your pencil?",
            acceptedAnswers: ["may i borrow your pencil", "may i borrow a pencil"]
          }
        ]
      },
      {
        id: 5,
        name: "The Final Escape Gate",
        shortName: "5. Final Escape Gate",
        mapCoords: { x: 91, y: 22 },
        sceneType: "final_gate",
        instruction: "FINAL CHALLENGE! Unlock all 3 golden locks to escape the jungle!",
        questions: [
          {
            promptHTML: "Lock 1: “Good morning, <span class='blank-highlight'>______</span>!”",
            targetWord: "Teacher",
            speakModel: "Teacher",
            acceptedAnswers: ["teacher", "good morning teacher"]
          },
          {
            promptHTML: "Lock 2: “Could you repeat that, please?”",
            targetWord: "Could you repeat that, please?",
            speakModel: "Could you repeat that, please?",
            acceptedAnswers: ["could you repeat that please", "could you repeat that"]
          },
          {
            promptHTML: "Lock 3: “Can you <span class='blank-highlight'>______</span> me, please?”",
            targetWord: "Help",
            speakModel: "Help",
            acceptedAnswers: ["help", "can you help me please", "can you help me"]
          }
        ]
      }
    ];

    /**
     * ========================================================================
     * 2. GAME STATE MANAGEMENT
     * ========================================================================
     */
    const totalQuestionsCount = CHECKPOINTS.reduce((sum, cp) => sum + cp.questions.length, 0);

    const gameState = {
      unlockedCheckpointIndex: 0, // 0 to 4
      currentCheckpointIndex: 0,
      currentQuestionIndex: 0,
      completedCheckpoints: [false, false, false, false, false],
      starsEarned: 0,
      coinsEarned: 0,
      questionSolved: false,
      isListening: false,
      soundEnabled: true,
      micPermissionGranted: false,
      micStream: null,
      micAnalyser: null,
      micMeterAnimId: null
    };

    const ENCOURAGING_SUCCESS = [
      "🌟 Amazing speaking, explorer!",
      "🎉 Great job! That was super clear!",
      "⭐ Fantastic pronunciation! Keep going!",
      "🏆 Brilliant English! Obstacle cleared!"
    ];

    /**
     * ========================================================================
     * 3. WEB AUDIO API SYNTHESIZER (NO EXTERNAL SOUND URLS NEEDED)
     * ========================================================================
     */
    let audioCtx = null;

    function initAudio() {
      if (!audioCtx) {
        const AudioContextClass = window.AudioContext || window.webkitAudioContext;
        if (AudioContextClass) {
          audioCtx = new AudioContextClass();
        }
      }
      if (audioCtx && audioCtx.state === "suspended") {
        audioCtx.resume();
      }
    }

    function playSoundEffect(type) {
      if (!gameState.soundEnabled) return;
      try {
        initAudio();
        if (!audioCtx) return;

        const now = audioCtx.currentTime;
        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();
        osc.connect(gain);
        gain.connect(audioCtx.destination);

        if (type === "click") {
          osc.type = "triangle";
          osc.frequency.setValueAtTime(520, now);
          osc.frequency.exponentialRampToValueAtTime(780, now + 0.08);
          gain.gain.setValueAtTime(0.15, now);
          gain.gain.exponentialRampToValueAtTime(0.001, now + 0.09);
          osc.start(now);
          osc.stop(now + 0.09);
        } else if (type === "success") {
          // Cheerful ascending major arpeggio
          const notes = [523.25, 659.25, 783.99, 1046.50];
          notes.forEach((freq, idx) => {
            const o = audioCtx.createOscillator();
            const g = audioCtx.createGain();
            o.type = "triangle";
            o.frequency.setValueAtTime(freq, now + idx * 0.09);
            g.gain.setValueAtTime(0.18, now + idx * 0.09);
            g.gain.exponentialRampToValueAtTime(0.001, now + idx * 0.09 + 0.35);
            o.connect(g);
            g.connect(audioCtx.destination);
            o.start(now + idx * 0.09);
            o.stop(now + idx * 0.09 + 0.35);
          });
        } else if (type === "retry") {
          osc.type = "sine";
          osc.frequency.setValueAtTime(320, now);
          osc.frequency.linearRampToValueAtTime(240, now + 0.25);
          gain.gain.setValueAtTime(0.15, now);
          gain.gain.exponentialRampToValueAtTime(0.001, now + 0.26);
          osc.start(now);
          osc.stop(now + 0.26);
        } else if (type === "victory") {
          const fanfare = [523.25, 659.25, 783.99, 1046.5, 783.99, 1046.5];
          fanfare.forEach((freq, idx) => {
            const o = audioCtx.createOscillator();
            const g = audioCtx.createGain();
            o.type = "triangle";
            o.frequency.setValueAtTime(freq, now + idx * 0.13);
            g.gain.setValueAtTime(0.2, now + idx * 0.13);
            g.gain.exponentialRampToValueAtTime(0.001, now + idx * 0.13 + 0.45);
            o.connect(g);
            g.connect(audioCtx.destination);
            o.start(now + idx * 0.13);
            o.stop(now + idx * 0.13 + 0.45);
          });
        }
      } catch (e) {
        // Ignore audio errors silently
      }
    }

    /**
     * ========================================================================
     * 4. TEXT-TO-SPEECH (MODEL PRONUNCIATION IN EN-GB)
     * ========================================================================
     */
    function speakModelWord(text) {
      if (!("speechSynthesis" in window)) {
        updateFeedbackPanel(
          "Text-to-speech is not supported in this browser.",
          "Please read the word aloud using the microphone!",
          "warning"
        );
        return;
      }

      // Cancel any ongoing speech first
      window.speechSynthesis.cancel();

      const utterance = new SpeechSynthesisUtterance(text);
      utterance.lang = "en-GB";
      utterance.rate = 0.88; // Slightly slower & clearer for Year 4 ESL learners
      utterance.pitch = 1.05;

      // Prefer a British English voice if available
      const voices = window.speechSynthesis.getVoices();
      const ukVoice = voices.find(v => v.lang.includes("en-GB")) || voices.find(v => v.lang.startsWith("en"));
      if (ukVoice) {
        utterance.voice = ukVoice;
      }

      window.speechSynthesis.speak(utterance);
    }

    // Ensure voices load asynchronously in Chrome/Edge
    if ("speechSynthesis" in window) {
      window.speechSynthesis.onvoiceschanged = () => window.speechSynthesis.getVoices();
    }

    /**
     * ========================================================================
     * 4B. EXPLICIT DEVICE MICROPHONE PERMISSION & LIVE AUDIO LEVEL METER
     * ========================================================================
     */
    async function requestAndMonitorMicrophone(isSetupTest = false) {
      const statusEl = document.getElementById("mic-setup-status");
      const hudMicBtn = document.getElementById("btn-hud-mic-check");

      if (!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia) {
        if (isSetupTest && statusEl) {
          statusEl.innerHTML = "⚠️ Your browser does not support microphone access. Please use <strong>Google Chrome</strong>, <strong>Edge</strong>, or <strong>Safari</strong>.";
          statusEl.style.color = "#b91c1c";
        }
        return false;
      }

      try {
        initAudio();
        if (!gameState.micStream) {
          gameState.micStream = await navigator.mediaDevices.getUserMedia({
            audio: {
              echoCancellation: true,
              noiseSuppression: true,
              autoGainControl: true
            }
          });
        }

        gameState.micPermissionGranted = true;
        if (hudMicBtn) {
          hudMicBtn.textContent = "✅ Mic Ready";
          hudMicBtn.classList.add("mic-ok");
        }

        if (isSetupTest && statusEl) {
          statusEl.innerHTML = "✅ <strong>Microphone Connected!</strong> Say “Hello!” — watch the green bar move!";
          statusEl.style.color = "#15803d";
        }

        // Connect stream to Web Audio API AnalyserNode for live volume visualization
        if (audioCtx && !gameState.micAnalyser) {
          const source = audioCtx.createMediaStreamSource(gameState.micStream);
          gameState.micAnalyser = audioCtx.createAnalyser();
          gameState.micAnalyser.fftSize = 256;
          source.connect(gameState.micAnalyser);
        }

        startLiveMicMeter();
        return true;
      } catch (err) {
        gameState.micPermissionGranted = false;
        let msg = "🚫 <strong>Microphone Permission Blocked!</strong> Click the 🔒 lock icon in your browser address bar, set Microphone to <strong>Allow</strong>, and try again.";

        if (err.name === "NotFoundError" || err.name === "DevicesNotFoundError") {
          msg = "🎧 <strong>No Microphone Found!</strong> Please plug in or turn on your device microphone.";
        } else if (window.self !== window.top) {
          msg = "🚫 <strong>Embedded Page Blocked Mic!</strong> Click <strong>Open Full Tab</strong> at the top of the game to use your microphone.";
          document.getElementById("iframe-mic-banner").style.display = "block";
        }

        if (isSetupTest && statusEl) {
          statusEl.innerHTML = msg;
          statusEl.style.color = "#b91c1c";
        } else {
          updateFeedbackPanel("🚫 Microphone Could Not Start", msg, "warning");
        }
        return false;
      }
    }

    function startLiveMicMeter() {
      if (!gameState.micAnalyser) return;
      if (gameState.micMeterAnimId) cancelAnimationFrame(gameState.micMeterAnimId);

      const dataArray = new Uint8Array(gameState.micAnalyser.frequencyBinCount);
      const setupFill = document.getElementById("setup-mic-meter-fill");
      const stageFill = document.getElementById("stage-mic-meter-fill");

      function updateMeter() {
        gameState.micAnalyser.getByteFrequencyData(dataArray);
        let sum = 0;
        for (let i = 0; i < dataArray.length; i++) {
          sum += dataArray[i];
        }
        const avg = sum / dataArray.length;
        const pct = Math.min(100, Math.round((avg / 65) * 100));

        if (setupFill) setupFill.style.width = `${pct}%`;
        if (stageFill) stageFill.style.width = `${pct}%`;

        gameState.micMeterAnimId = requestAnimationFrame(updateMeter);
      }
      updateMeter();
    }

    /**
     * ========================================================================
     * 5. WEB SPEECH API (SPEECH RECOGNITION EN-GB)
     * ========================================================================
     */
    const SpeechRecognitionAPI = window.SpeechRecognition || window.webkitSpeechRecognition;
    let recognition = null;

    if (SpeechRecognitionAPI) {
      recognition = new SpeechRecognitionAPI();
      recognition.lang = "en-GB";
      recognition.interimResults = true; // Real-time live speech display
      recognition.maxAlternatives = 3;
      recognition.continuous = false;
    }

    /**
     * Normalizes spoken text and expected answers by removing punctuation,
     * extra spaces, and converting to lowercase for fair matching.
     */
    function normalizeSpeechText(str) {
      return str
        .toLowerCase()
        .replace(/[.,\/#!$%\^&\*;:{}=\-_`~()?'"’“”]/g, "")
        .replace(/\s+/g, " ")
        .trim();
    }

    async function startSpeechRecognition() {
      if (gameState.questionSolved) return;

      // Check if Web Speech API is supported by the pupil's browser
      if (!recognition) {
        updateFeedbackPanel(
          "⚠️ Speech Recognition is not supported in this browser.",
          "Please open this game in Google Chrome, Microsoft Edge, or Safari and allow microphone permission.",
          "warning"
        );
        return;
      }

      // Stop any TTS playback before listening so the mic doesn't hear the computer
      if ("speechSynthesis" in window) {
        window.speechSynthesis.cancel();
      }

      if (gameState.isListening) {
        try {
          recognition.stop();
        } catch (e) {}
        return;
      }

      // Explicitly unlock & verify device microphone permission before starting SpeechRecognition
      if (!gameState.micPermissionGranted) {
        const micOk = await requestAndMonitorMicrophone(false);
        if (!micOk) return;
      }

      try {
        recognition.start();
      } catch (err) {
        updateFeedbackPanel(
          "⚠️ Microphone is busy or starting up.",
          "Wait 1 second and click SPEAK NOW again.",
          "warning"
        );
      }
    }

    if (recognition) {
      recognition.onstart = () => {
        gameState.isListening = true;
        const micBtn = document.getElementById("btn-speak");
        micBtn.classList.add("listening");
        document.getElementById("mic-btn-text").textContent = "LISTENING... SPEAK!";
        document.getElementById("btn-retry").style.display = "none";
        document.getElementById("stage-mic-meter").classList.add("active");

        updateFeedbackPanel(
          "🎙️ Microphone ON! Speak clearly into your device now...",
          "",
          ""
        );
      };

      recognition.onresult = (event) => {
        const lastResultIndex = event.results.length - 1;
        const resultItem = event.results[lastResultIndex];
        const primaryTranscript = resultItem[0].transcript;

        // Show live interim words while the student is still speaking
        if (!resultItem.isFinal) {
          updateFeedbackPanel(
            `🎙️ Hearing: “${primaryTranscript}...”`,
            "Keep speaking clearly!",
            ""
          );
          return;
        }

        // Collect all alternative interpretations from Web Speech API
        const alternatives = [];
        for (let i = 0; i < resultItem.length; i++) {
          alternatives.push(normalizeSpeechText(resultItem[i].transcript));
        }

        const cp = CHECKPOINTS[gameState.currentCheckpointIndex];
        const q = cp.questions[gameState.currentQuestionIndex];
        const normalizedTarget = normalizeSpeechText(q.targetWord);

        // Check if any recognized alternative matches accepted answers
        const isMatch = alternatives.some(spoken => {
          if (spoken === normalizedTarget) return true;
          if (q.acceptedAnswers.includes(spoken)) return true;
          // Also allow if the pupil spoke the full sentence containing the target word
          if (q.acceptedAnswers.some(ans => spoken.includes(ans) && ans.length >= 4)) return true;
          return false;
        });

        if (isMatch) {
          handleCorrectPronunciation(primaryTranscript);
        } else {
          handleIncorrectPronunciation(primaryTranscript);
        }
      };

      recognition.onerror = (event) => {
        gameState.isListening = false;
        resetMicButtonUI();

        let friendlyTitle = "⚠️ We couldn't hear you clearly.";
        let friendlyTip = "Please check your microphone permission and try speaking louder!";

        if (event.error === "not-allowed" || event.error === "service-not-allowed") {
          friendlyTitle = "🚫 Microphone Permission Blocked!";
          friendlyTip = "Click the 🔒 lock icon in your browser address bar, allow Microphone access, and press Retry.";
          if (window.self !== window.top) {
            document.getElementById("iframe-mic-banner").style.display = "block";
          }
        } else if (event.error === "audio-capture") {
          friendlyTitle = "🎧 No Microphone Detected!";
          friendlyTip = "Check that your device microphone is connected and not muted.";
        } else if (event.error === "no-speech") {
          friendlyTitle = "🤫 No voice detected!";
          friendlyTip = "Press SPEAK NOW and say the English word clearly into your microphone.";
        } else if (event.error === "network") {
          friendlyTitle = "🌐 Network connection needed for speech recognition.";
          friendlyTip = "Check your internet connection and press Retry.";
        }

        updateFeedbackPanel(friendlyTitle, friendlyTip, "warning");
        document.getElementById("btn-retry").style.display = "inline-flex";
      };

      recognition.onend = () => {
        gameState.isListening = false;
        resetMicButtonUI();
      };
    }

    function resetMicButtonUI() {
      const micBtn = document.getElementById("btn-speak");
      micBtn.classList.remove("listening");
      document.getElementById("mic-btn-text").textContent = "SPEAK NOW";
      document.getElementById("stage-mic-meter").classList.remove("active");
    }

    /**
     * ========================================================================
     * 6. HANDLING CORRECT & INCORRECT ANSWERS
     * ========================================================================
     */
    function handleCorrectPronunciation(spokenText) {
      gameState.questionSolved = true;
      gameState.starsEarned += 1;
      gameState.coinsEarned += 10;

      playSoundEffect("success");
      updateHUD();

      const praise = ENCOURAGING_SUCCESS[Math.floor(Math.random() * ENCOURAGING_SUCCESS.length)];
      updateFeedbackPanel(
        `✅ You said: “${spokenText}”`,
        `${praise} (+1 ⭐ Star & +10 🪙 Coins)`,
        "success"
      );

      // Update stage animation (e.g., open gate, raise stepping stone, repair bridge plank)
      renderCheckpointVisualScene(true);

      // Mark current question dot as complete
      const dots = document.querySelectorAll(".q-dot");
      if (dots[gameState.currentQuestionIndex]) {
        dots[gameState.currentQuestionIndex].classList.remove("current");
        dots[gameState.currentQuestionIndex].classList.add("done");
      }

      document.getElementById("btn-retry").style.display = "none";
      const nextBtn = document.getElementById("btn-next-question");
      const cp = CHECKPOINTS[gameState.currentCheckpointIndex];

      if (gameState.currentQuestionIndex < cp.questions.length - 1) {
        nextBtn.innerHTML = "⭐ NEXT CHALLENGE ➔";
      } else if (gameState.currentCheckpointIndex < CHECKPOINTS.length - 1) {
        nextBtn.innerHTML = "🚪 UNLOCK CHECKPOINT & CONTINUE ➔";
      } else {
        nextBtn.innerHTML = "🏆 OPEN FINAL ESCAPE GATE! ➔";
      }

      nextBtn.style.display = "inline-flex";
    }

    function handleIncorrectPronunciation(spokenText) {
      playSoundEffect("retry");

      updateFeedbackPanel(
        `🗣️ You said: “${spokenText}”`,
        "Keep trying, explorer! Click LISTEN to hear it again, then press SPEAK NOW.",
        "error"
      );

      document.getElementById("btn-retry").style.display = "inline-flex";
    }

    function updateFeedbackPanel(topLine, bottomLine, statusClass) {
      const panel = document.getElementById("speech-panel");
      const recEl = document.getElementById("recognized-display");
      const fbEl = document.getElementById("feedback-display");

      panel.className = "speech-status-panel";
      fbEl.className = "feedback-msg";

      if (statusClass) {
        panel.classList.add(statusClass);
        fbEl.classList.add(statusClass);
      }

      recEl.innerHTML = topLine;
      fbEl.innerHTML = bottomLine;
    }

    /**
     * ========================================================================
     * 7. SCREEN NAVIGATION & JUNGLE MAP RENDERING
     * ========================================================================
     */
    function showScreen(screenId) {
      document.querySelectorAll(".screen").forEach(s => s.classList.remove("active"));
      document.getElementById(screenId).classList.add("active");

      const hud = document.getElementById("hud-bar");
      if (screenId === "map-screen" || screenId === "checkpoint-screen") {
        hud.style.display = "flex";
      } else {
        hud.style.display = "none";
      }
    }

    function updateHUD() {
      document.getElementById("hud-stars").textContent = gameState.starsEarned;
      document.getElementById("hud-total-stars").textContent = totalQuestionsCount;
      document.getElementById("hud-coins").textContent = gameState.coinsEarned;

      const pct = Math.round((gameState.starsEarned / totalQuestionsCount) * 100);
      document.getElementById("hud-progress-pct").textContent = `${pct}%`;
      document.getElementById("hud-progress-fill").style.width = `${pct}%`;
    }

    function renderJungleMap() {
      const container = document.getElementById("map-nodes-container");
      container.innerHTML = "";

      CHECKPOINTS.forEach((cp, index) => {
        const node = document.createElement("div");
        node.className = "map-node";
        node.style.left = `${cp.mapCoords.x}%`;
        node.style.top = `${cp.mapCoords.y}%`;

        let iconText = cp.id;
        if (gameState.completedCheckpoints[index]) {
          node.classList.add("completed");
          iconText = "⭐";
        } else if (index === gameState.unlockedCheckpointIndex) {
          node.classList.add("active");
          iconText = cp.id;
        } else {
          node.classList.add("locked");
          iconText = "🔒";
        }

        node.innerHTML = `
          <div class="node-circle">${iconText}</div>
          <div class="node-label">${cp.shortName}</div>
        `;

        node.addEventListener("click", () => {
          if (index <= gameState.unlockedCheckpointIndex) {
            playSoundEffect("click");
            startCheckpoint(index);
          } else {
            playSoundEffect("retry");
          }
        });

        container.appendChild(node);
      });

      // Position Explorer Avatar at current active checkpoint
      const targetCp = CHECKPOINTS[gameState.unlockedCheckpointIndex];
      const avatar = document.getElementById("map-explorer-avatar");
      avatar.style.left = `${targetCp.mapCoords.x}%`;
      avatar.style.top = `${targetCp.mapCoords.y}%`;

      // Update Map CTA Button
      const activeCp = CHECKPOINTS[gameState.unlockedCheckpointIndex];
      const enterBtn = document.getElementById("btn-enter-active-checkpoint");
      enterBtn.innerHTML = `🚀 ENTER CHECKPOINT ${activeCp.id}: ${activeCp.name.toUpperCase()}`;
      document.getElementById("map-status-text").textContent =
        `Next Obstacle: Checkpoint ${activeCp.id} — ${activeCp.name}`;
    }

    /**
     * ========================================================================
     * 8. CHECKPOINT STAGE LOADING & ANIMATED SVG ILLUSTRATIONS
     * ========================================================================
     */
    function startCheckpoint(cpIndex) {
      gameState.currentCheckpointIndex = cpIndex;
      gameState.currentQuestionIndex = 0;
      showScreen("checkpoint-screen");
      loadCurrentQuestion();
    }

    function loadCurrentQuestion() {
      const cp = CHECKPOINTS[gameState.currentCheckpointIndex];
      const q = cp.questions[gameState.currentQuestionIndex];
      gameState.questionSolved = false;

      // Update Header & Instruction
      document.getElementById("stage-header-title").textContent =
        `Checkpoint ${cp.id}: ${cp.name}`;
      document.getElementById("challenge-instruction").textContent = cp.instruction;

      // Render Question Progress Dots
      const dotsContainer = document.getElementById("question-progress-dots");
      dotsContainer.innerHTML = "";
      cp.questions.forEach((_, idx) => {
        const dot = document.createElement("div");
        dot.className = "q-dot";
        if (idx < gameState.currentQuestionIndex) dot.classList.add("done");
        if (idx === gameState.currentQuestionIndex) dot.classList.add("current");
        dotsContainer.appendChild(dot);
      });

      // Update Sentence & Expected Word Display
      document.getElementById("prompt-display").innerHTML = q.promptHTML;
      document.getElementById("expected-answer-display").textContent = q.targetWord;

      // Reset Controls & Feedback
      document.getElementById("btn-next-question").style.display = "none";
      document.getElementById("btn-retry").style.display = "none";
      resetMicButtonUI();

      updateFeedbackPanel(
        "Press <strong>LISTEN</strong> to hear the pronunciation, then press <strong>SPEAK NOW</strong>!",
        "",
        ""
      );

      // Render Stage Artwork
      renderCheckpointVisualScene(false);

      // Checkpoint 3 (The Echoing Cave) plays the echo automatically as requested
      if (cp.autoPlayAudio) {
        setTimeout(() => speakModelWord(q.speakModel), 450);
      }
    }

    /**
     * Renders custom interactive SVG illustrations for each of the 5 Checkpoints,
     * dynamically animating obstacles opening/repairing as the pupil solves questions!
     */
    function renderCheckpointVisualScene(justSolved) {
      const cp = CHECKPOINTS[gameState.currentCheckpointIndex];
      const qIdx = gameState.currentQuestionIndex;
      const totalQ = cp.questions.length;
      const solvedCount = justSolved ? qIdx + 1 : qIdx;
      const box = document.getElementById("stage-visual-box");

      if (cp.sceneType === "gate") {
        // CHECKPOINT 1: THE WHISPERING JUNGLE (Wooden Gate opens when stage is solved)
        const gateOpen = justSolved && qIdx === totalQ - 1;
        const explorerX = gateOpen ? 540 : 250 + solvedCount * 22;

        box.innerHTML = `
          <svg viewBox="0 0 880 185" preserveAspectRatio="none" style="width:100%; height:100%;">
            <rect width="880" height="185" fill="#86efac"/>
            <ellipse cx="180" cy="170" rx="220" ry="60" fill="#22c55e"/>
            <ellipse cx="700" cy="170" rx="240" ry="65" fill="#16a34a"/>
            <!-- Jungle Trees -->
            <rect x="70" y="30" width="22" height="130" fill="#783f12"/>
            <circle cx="81" cy="35" r="45" fill="#15803d"/>
            <rect x="770" y="25" width="24" height="135" fill="#783f12"/>
            <circle cx="782" cy="32" r="48" fill="#15803d"/>
            <!-- Dirt Path -->
            <rect x="0" y="145" width="880" height="40" fill="#d97706"/>
            <!-- Wooden Gate -->
            <g transform="translate(440, 35)">
              <rect x="-15" y="0" width="16" height="125" rx="4" fill="#451a03"/>
              <rect x="95" y="0" width="16" height="125" rx="4" fill="#451a03"/>
              ${gateOpen
                ? `<text x="48" y="65" text-anchor="middle" font-family="Fredoka" font-size="22" fill="#14532d" font-weight="bold">✨ GATE OPEN! ✨</text>`
                : `<g>
                     <rect x="2" y="15" width="92" height="95" rx="6" fill="#92400e" stroke="#451a03" stroke-width="3"/>
                     <line x1="25" y1="15" x2="25" y2="110" stroke="#451a03" stroke-width="3"/>
                     <line x1="48" y1="15" x2="48" y2="110" stroke="#451a03" stroke-width="3"/>
                     <line x1="71" y1="15" x2="71" y2="110" stroke="#451a03" stroke-width="3"/>
                     <circle cx="48" cy="62" r="14" fill="#fbbf24" stroke="#451a03" stroke-width="3"/>
                     <text x="48" y="67" text-anchor="middle" font-size="14">🔒</text>
                   </g>`
              }
            </g>
            <!-- Explorer -->
            <g transform="translate(${explorerX}, 68)" style="transition: transform 0.6s ease;">
              ${getMiniExplorerSVG()}
            </g>
          </svg>
        `;
      } else if (cp.sceneType === "river") {
        // CHECKPOINT 2: THE CROCODILE RIVER (Stepping stones rise per question)
        const explorerX = 210 + solvedCount * 115;
        box.innerHTML = `
          <svg viewBox="0 0 880 185" preserveAspectRatio="none" style="width:100%; height:100%;">
            <rect width="880" height="185" fill="#bbf7d0"/>
            <!-- Left & Right Riverbanks -->
            <rect x="0" y="130" width="270" height="55" fill="#16a34a"/>
            <rect x="610" y="130" width="270" height="55" fill="#16a34a"/>
            <!-- Blue River -->
            <rect x="260" y="132" width="360" height="53" fill="#0284c7"/>
            <path d="M275,148 Q330,140 385,148 T495,148 T605,148" stroke="#bae6fd" stroke-width="4" fill="none"/>
            <!-- Stepping Stones (Appear as questions are solved) -->
            ${[0, 1, 2].map(i => {
              const active = i < solvedCount;
              const sx = 315 + i * 105;
              return `<ellipse cx="${sx}" cy="146" rx="34" ry="12"
                fill="${active ? '#cbd5e1' : '#0369a1'}"
                stroke="${active ? '#475569' : '#0c4a6e'}" stroke-width="3"
                opacity="${active ? '1' : '0.45'}"/>`;
            }).join("")}
            <!-- Cartoon Crocodile -->
            <g transform="translate(420, 92)" style="animation: floatBob 2s ease-in-out infinite;">
              <ellipse cx="65" cy="36" rx="58" ry="16" fill="#15803d" stroke="#052e16" stroke-width="3"/>
              <path d="M115,32 L155,24 L152,36 L115,40 Z" fill="#22c55e" stroke="#052e16" stroke-width="2.5"/>
              <path d="M115,40 L150,46 L148,38 Z" fill="#16a34a" stroke="#052e16" stroke-width="2"/>
              <circle cx="105" cy="22" r="9" fill="#22c55e" stroke="#052e16" stroke-width="2.5"/>
              <circle cx="107" cy="21" r="4" fill="#fef08a"/>
              <circle cx="108" cy="21" r="2" fill="#000"/>
              <polygon points="35,21 43,12 51,21" fill="#4ade80"/>
              <polygon points="55,20 63,11 71,20" fill="#4ade80"/>
              <polygon points="75,21 83,12 91,21" fill="#4ade80"/>
            </g>
            <!-- Explorer -->
            <g transform="translate(${explorerX}, 56)" style="transition: transform 0.6s ease;">
              ${getMiniExplorerSVG()}
            </g>
          </svg>
        `;
      } else if (cp.sceneType === "cave") {
        // CHECKPOINT 3: THE ECHOING CAVE (Glowing crystals & bouncing echo waves)
        box.innerHTML = `
          <svg viewBox="0 0 880 185" preserveAspectRatio="none" style="width:100%; height:100%;">
            <rect width="880" height="185" fill="#1e1b4b"/>
            <!-- Cave Arch & Stalactites -->
            <path d="M0,185 L0,40 Q440,-20 880,40 L880,185 Z" fill="#312e81"/>
            <polygon points="120,20 140,75 160,18" fill="#4338ca"/>
            <polygon points="710,18 730,80 750,20" fill="#4338ca"/>
            <!-- Glowing Crystals -->
            <polygon points="160,155 178,95 196,155" fill="#38bdf8" stroke="#bae6fd" stroke-width="2"/>
            <polygon points="188,155 202,115 215,155" fill="#818cf8" stroke="#c7d2fe" stroke-width="2"/>
            <polygon points="670,155 688,92 706,155" fill="#f472b6" stroke="#fbcfe8" stroke-width="2"/>
            <!-- Echo Soundwave Rings -->
            <circle cx="520" cy="88" r="28" fill="none" stroke="#38bdf8" stroke-width="4" opacity="0.85"/>
            <circle cx="520" cy="88" r="50" fill="none" stroke="#a5f3fc" stroke-width="3" stroke-dasharray="10 8" opacity="0.7"/>
            <circle cx="520" cy="88" r="74" fill="none" stroke="#c084fc" stroke-width="2.5" stroke-dasharray="12 10" opacity="0.55"/>
            <text x="520" y="94" text-anchor="middle" font-family="Fredoka" font-size="18" fill="#fde047" font-weight="bold">
              🔊 ECHO...
            </text>
            <!-- Cave Floor -->
            <rect x="0" y="150" width="880" height="35" fill="#1e293b"/>
            <!-- Explorer -->
            <g transform="translate(290, 72)">
              ${getMiniExplorerSVG()}
            </g>
          </svg>
        `;
      } else if (cp.sceneType === "bridge") {
        // CHECKPOINT 4: THE BROKEN BRIDGE (4 wooden planks get repaired one by one)
        const explorerX = solvedCount === totalQ ? 660 : 175 + solvedCount * 35;
        box.innerHTML = `
          <svg viewBox="0 0 880 185" preserveAspectRatio="none" style="width:100%; height:100%;">
            <rect width="880" height="185" fill="#bae6fd"/>
            <!-- Left & Right Cliffs -->
            <rect x="0" y="120" width="240" height="65" fill="#15803d"/>
            <rect x="640" y="120" width="240" height="65" fill="#15803d"/>
            <!-- Deep Jungle Canyon -->
            <rect x="240" y="135" width="400" height="50" fill="#064e3b"/>
            <!-- Bridge Ropes -->
            <path d="M235,85 Q440,115 645,85" stroke="#783f12" stroke-width="5" fill="none"/>
            <path d="M235,122 Q440,132 645,122" stroke="#783f12" stroke-width="6" fill="none"/>
            <!-- 4 Wooden Planks (Repaired as each expression is spoken) -->
            ${[0, 1, 2, 3].map(i => {
              const repaired = i < solvedCount;
              const px = 265 + i * 92;
              return repaired
                ? `<rect x="${px}" y="114" width="76" height="16" rx="4" fill="#d97706" stroke="#451a03" stroke-width="3"/>`
                : `<rect x="${px}" y="116" width="76" height="14" rx="4" fill="none" stroke="#ef4444" stroke-width="2.5" stroke-dasharray="6 5"/>`;
            }).join("")}
            <!-- Explorer -->
            <g transform="translate(${explorerX}, 44)" style="transition: transform 0.6s ease;">
              ${getMiniExplorerSVG()}
            </g>
          </svg>
        `;
      } else {
        // CHECKPOINT 5: THE FINAL ESCAPE GATE (3 Golden Locks unlock one by one)
        const allUnlocked = solvedCount === totalQ;
        box.innerHTML = `
          <svg viewBox="0 0 880 185" preserveAspectRatio="none" style="width:100%; height:100%;">
            <rect width="880" height="185" fill="#fef08a"/>
            <ellipse cx="440" cy="180" rx="460" ry="70" fill="#4ade80"/>
            <!-- Grand Golden Temple Exit -->
            <rect x="310" y="22" width="260" height="135" rx="14" fill="${allUnlocked ? '#fef9c3' : '#b45309'}" stroke="#783f12" stroke-width="5"/>
            <rect x="290" y="10" width="300" height="24" rx="8" fill="#fbbf24" stroke="#783f12" stroke-width="4"/>
            <text x="440" y="27" text-anchor="middle" font-family="Fredoka" font-size="14" fill="#451a03" font-weight="bold">
              HOME EXIT GATE
            </text>
            <!-- 3 Golden Locks -->
            ${[0, 1, 2].map(i => {
              const unlocked = i < solvedCount;
              const lx = 365 + i * 75;
              return `
                <g transform="translate(${lx}, 85)">
                  <circle cx="0" cy="0" r="24" fill="${unlocked ? '#22c55e' : '#fbbf24'}" stroke="#451a03" stroke-width="3"/>
                  <text x="0" y="8" text-anchor="middle" font-size="22">${unlocked ? '🔓' : '🔒'}</text>
                </g>
              `;
            }).join("")}
            <!-- Ground -->
            <rect x="0" y="152" width="880" height="33" fill="#16a34a"/>
            <!-- Explorer -->
            <g transform="translate(${allUnlocked ? 410 : 190}, 75)" style="transition: transform 0.6s ease;">
              ${getMiniExplorerSVG()}
            </g>
          </svg>
        `;
      }
    }

    function getMiniExplorerSVG() {
      return `
        <rect x="8" y="34" width="44" height="34" rx="10" fill="#d97706" stroke="#451a03" stroke-width="2.5"/>
        <rect x="16" y="36" width="28" height="34" rx="8" fill="#65a30d" stroke="#365314" stroke-width="2.5"/>
        <circle cx="30" cy="22" r="16" fill="#fed7aa" stroke="#783f12" stroke-width="2.5"/>
        <circle cx="25" cy="21" r="2.3" fill="#1e293b"/>
        <circle cx="35" cy="21" r="2.3" fill="#1e293b"/>
        <path d="M25,27 Q30,32 35,27" stroke="#783f12" stroke-width="2" fill="none" stroke-linecap="round"/>
        <ellipse cx="30" cy="9" rx="23" ry="5" fill="#d97706" stroke="#451a03" stroke-width="2"/>
        <path d="M16,9 Q16,0 30,0 Q44,0 44,9 Z" fill="#f59e0b" stroke="#451a03" stroke-width="2"/>
      `;
    }

    /**
     * ========================================================================
     * 9. ADVANCING QUESTIONS & VICTORY CELEBRATION
     * ========================================================================
     */
    function handleNextQuestionClick() {
      playSoundEffect("click");
      const cp = CHECKPOINTS[gameState.currentCheckpointIndex];

      if (gameState.currentQuestionIndex < cp.questions.length - 1) {
        // Move to next question inside the same checkpoint
        gameState.currentQuestionIndex += 1;
        loadCurrentQuestion();
      } else {
        // Checkpoint completed!
        gameState.completedCheckpoints[gameState.currentCheckpointIndex] = true;

        if (gameState.currentCheckpointIndex < CHECKPOINTS.length - 1) {
          // Unlock next checkpoint and return to map
          gameState.unlockedCheckpointIndex = Math.max(
            gameState.unlockedCheckpointIndex,
            gameState.currentCheckpointIndex + 1
          );
          renderJungleMap();
          showScreen("map-screen");
        } else {
          // All 5 Checkpoints completed -> Show Victory Screen!
          triggerVictoryScreen();
        }
      }
    }

    function triggerVictoryScreen() {
      playSoundEffect("victory");
      document.getElementById("final-stars-text").textContent =
        `⭐ ${gameState.starsEarned} / ${totalQuestionsCount}`;
      document.getElementById("final-coins-text").textContent =
        `🪙 ${gameState.coinsEarned}`;

      showScreen("victory-screen");
      launchConfetti();
    }

    function resetEntireGame() {
      playSoundEffect("click");
      gameState.unlockedCheckpointIndex = 0;
      gameState.currentCheckpointIndex = 0;
      gameState.currentQuestionIndex = 0;
      gameState.completedCheckpoints = [false, false, false, false, false];
      gameState.starsEarned = 0;
      gameState.coinsEarned = 0;
      gameState.questionSolved = false;

      updateHUD();
      renderJungleMap();
      showScreen("welcome-screen");
    }

    /**
     * ========================================================================
     * 10. VICTORY CONFETTI ANIMATION
     * ========================================================================
     */
    function launchConfetti() {
      const canvas = document.getElementById("confetti-canvas");
      const ctx = canvas.getContext("2d");
      canvas.width = canvas.parentElement.clientWidth;
      canvas.height = canvas.parentElement.clientHeight;

      const colors = ["#fbbf24", "#22c55e", "#38bdf8", "#f43f5e", "#a855f7", "#fb923c"];
      const pieces = Array.from({ length: 95 }, () => ({
        x: Math.random() * canvas.width,
        y: Math.random() * canvas.height - canvas.height,
        w: Math.random() * 10 + 6,
        h: Math.random() * 6 + 4,
        color: colors[Math.floor(Math.random() * colors.length)],
        speedY: Math.random() * 3 + 2,
        speedX: Math.random() * 2 - 1,
        angle: Math.random() * 360
      }));

      let frames = 0;
      function animateConfetti() {
        if (!document.getElementById("victory-screen").classList.contains("active")) return;
        ctx.clearRect(0, 0, canvas.width, canvas.height);

        pieces.forEach(p => {
          p.y += p.speedY;
          p.x += p.speedX;
          p.angle += 4;
          if (p.y > canvas.height) p.y = -10;

          ctx.save();
          ctx.translate(p.x, p.y);
          ctx.rotate((p.angle * Math.PI) / 180);
          ctx.fillStyle = p.color;
          ctx.fillRect(-p.w / 2, -p.h / 2, p.w, p.h);
          ctx.restore();
        });

        frames++;
        if (frames < 420) {
          requestAnimationFrame(animateConfetti);
        }
      }
      animateConfetti();
    }

    /**
     * ========================================================================
     * 11. EVENT LISTENERS & INITIALIZATION
     * ========================================================================
     */
    document.getElementById("btn-start-journey").addEventListener("click", () => {
      playSoundEffect("click");
      showScreen("instruction-screen");
    });

    document.getElementById("btn-test-mic").addEventListener("click", () => {
      playSoundEffect("click");
      requestAndMonitorMicrophone(true);
    });

    document.getElementById("btn-hud-mic-check").addEventListener("click", () => {
      playSoundEffect("click");
      showScreen("instruction-screen");
      requestAndMonitorMicrophone(true);
    });

    document.getElementById("btn-to-jungle-map").addEventListener("click", () => {
      playSoundEffect("click");
      updateHUD();
      renderJungleMap();
      showScreen("map-screen");
    });

    document.getElementById("btn-enter-active-checkpoint").addEventListener("click", () => {
      playSoundEffect("click");
      startCheckpoint(gameState.unlockedCheckpointIndex);
    });

    document.getElementById("btn-to-map").addEventListener("click", () => {
      playSoundEffect("click");
      renderJungleMap();
      showScreen("map-screen");
    });

    document.getElementById("btn-listen").addEventListener("click", () => {
      if (gameState.isListening && recognition) {
        try { recognition.stop(); } catch (e) {}
      }
      const cp = CHECKPOINTS[gameState.currentCheckpointIndex];
      const q = cp.questions[gameState.currentQuestionIndex];
      speakModelWord(q.speakModel);
    });

    document.getElementById("btn-speak").addEventListener("click", () => {
      startSpeechRecognition();
    });

    document.getElementById("btn-retry").addEventListener("click", () => {
      startSpeechRecognition();
    });

    document.getElementById("btn-next-question").addEventListener("click", () => {
      handleNextQuestionClick();
    });

    document.getElementById("btn-play-again").addEventListener("click", () => {
      resetEntireGame();
    });

    document.getElementById("btn-sound-toggle").addEventListener("click", (e) => {
      gameState.soundEnabled = !gameState.soundEnabled;
      e.target.textContent = gameState.soundEnabled ? "🔊 SFX On" : "🔇 SFX Off";
    });

    // Initialize HUD, Map & Check Iframe Embedding on load
    window.addEventListener("load", () => {
      updateHUD();
      renderJungleMap();

      // Detect if embedded in an iframe (such as Google Sites) and check mic permission state
      if (window.self !== window.top) {
        const fullTabBtn = document.getElementById("btn-open-full-tab");
        if (fullTabBtn) fullTabBtn.href = window.location.href;

        if (navigator.permissions && navigator.permissions.query) {
          navigator.permissions.query({ name: "microphone" }).then(permStatus => {
            if (permStatus.state === "denied") {
              document.getElementById("iframe-mic-banner").style.display = "block";
            }
          }).catch(() => {});
        }
      }
    });
  </script>
</body>
</html>
