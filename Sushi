import { useState, useEffect, useRef } from "react";

const CSS = `
@import url('https://fonts.googleapis.com/css2?family=Pacifico&family=Nunito:wght@400;600;700;800;900&family=Dancing+Script:wght@600;700&display=swap');

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
::-webkit-scrollbar { width: 6px; }
::-webkit-scrollbar-track { background: #fff0f5; }
::-webkit-scrollbar-thumb { background: linear-gradient(#FF6B9D, #C77DFF); border-radius: 10px; }

@keyframes gradientShift {
  0%,100% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
}
@keyframes blobMorph {
  0%,100% { border-radius: 60% 40% 30% 70% / 60% 30% 70% 40%; }
  25% { border-radius: 40% 60% 70% 30% / 40% 70% 30% 60%; }
  50% { border-radius: 30% 60% 70% 40% / 50% 60% 30% 60%; }
  75% { border-radius: 70% 30% 50% 60% / 30% 50% 70% 40%; }
}
@keyframes floatUp {
  0% { transform: translateY(100vh) scale(0.5) rotate(0deg); opacity: 0; }
  10% { opacity: 1; }
  90% { opacity: 0.8; }
  100% { transform: translateY(-120px) scale(1.3) rotate(25deg); opacity: 0; }
}
@keyframes floatBob {
  0%,100% { transform: translateY(0px) rotate(-3deg); }
  50% { transform: translateY(-16px) rotate(3deg); }
}
@keyframes pulse {
  0%,100% { transform: scale(1); box-shadow: 0 0 0 0 rgba(255,107,157,0.5); }
  50% { transform: scale(1.05); box-shadow: 0 0 0 14px rgba(255,107,157,0); }
}
@keyframes wiggle {
  0%,100% { transform: rotate(-5deg); }
  50% { transform: rotate(5deg); }
}
@keyframes bounce {
  0%,100% { transform: translateY(0); }
  40% { transform: translateY(-14px); }
  65% { transform: translateY(-7px); }
}
@keyframes popIn {
  0% { transform: scale(0) rotate(-10deg); opacity: 0; }
  65% { transform: scale(1.12) rotate(2deg); }
  100% { transform: scale(1) rotate(0deg); opacity: 1; }
}
@keyframes fall {
  from { transform: translateY(-60px); opacity: 1; }
  to { transform: translateY(430px); opacity: 0.3; }
}
@keyframes heartbeat {
  0%,100% { transform: scale(1); }
  14% { transform: scale(1.15); }
  28% { transform: scale(1); }
  42% { transform: scale(1.1); }
  70% { transform: scale(1); }
}
@keyframes shimmer {
  0% { background-position: -300% center; }
  100% { background-position: 300% center; }
}

.glass {
  background: rgba(255,255,255,0.28);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border: 1px solid rgba(255,255,255,0.58);
  box-shadow: 0 8px 32px rgba(255,150,180,0.14);
}
.glass-pink {
  background: rgba(255,182,193,0.18);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border: 1px solid rgba(255,182,193,0.5);
  box-shadow: 0 8px 40px rgba(255,107,157,0.18);
}
.gradient-text {
  background: linear-gradient(135deg, #FF6B9D 0%, #C77DFF 50%, #FF9E7E 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}
.shimmer-text {
  background: linear-gradient(90deg, #FF6B9D, #C77DFF, #FF9E7E, #FF6B9D);
  background-size: 300% auto;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  animation: shimmer 4s linear infinite;
}
.glow-pink { box-shadow: 0 0 28px rgba(255,107,157,0.45), 0 0 56px rgba(255,107,157,0.18); }
.lift {
  transition: transform 0.32s cubic-bezier(.34,1.56,.64,1), box-shadow 0.32s ease;
  cursor: pointer;
}
.lift:hover {
  transform: translateY(-9px) scale(1.02);
  box-shadow: 0 24px 52px rgba(255,107,157,0.28);
}
.mood-btn {
  transition: all 0.25s cubic-bezier(.34,1.56,.64,1);
  cursor: pointer;
}
.mood-btn:hover { transform: scale(1.14) rotate(-2deg); }
.mood-btn.selected { transform: scale(1.18); box-shadow: 0 0 26px rgba(255,107,157,0.55); }
.hug-btn {
  animation: pulse 2.5s ease-in-out infinite;
  cursor: pointer;
  transition: all 0.2s ease;
}
.hug-btn:active { transform: scale(0.93) !important; }
.floating-bg {
  position: fixed; pointer-events: none; z-index: 0;
  animation: floatUp linear forwards;
}
.falling-candy {
  position: absolute; cursor: pointer; user-select: none;
  font-size: 34px; z-index: 2;
  animation: fall linear forwards;
  transition: opacity 0.15s, transform 0.15s;
}
.falling-candy:active { transform: scale(0) !important; opacity: 0 !important; }
.section-divider {
  text-align: center; font-size: 20px;
  letter-spacing: 16px; opacity: 0.4; margin: 4px 0;
  position: relative; z-index: 1;
}
.card-grid-3 {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(230px, 1fr));
  gap: 18px;
}
.card-grid-4 {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(190px, 1fr));
  gap: 14px;
}
@media (max-width: 560px) {
  .card-grid-3 { grid-template-columns: 1fr 1fr; gap: 11px; }
  .card-grid-4 { grid-template-columns: 1fr 1fr; gap: 10px; }
}
`;

/* ═══════════════════════════════════════════════════════════
   FLOATING BACKGROUND ELEMENTS
═══════════════════════════════════════════════════════════ */
const BG_EMOJIS = ['💗','✨','🌸','💕','⭐','🌟','💖','🫧','🌺','💫','🎀','🍬'];

function FloatingBg() {
  const [items, setItems] = useState([]);
  useEffect(() => {
    const id = setInterval(() => {
      setItems(p => [...p.slice(-22), {
        id: Date.now() + Math.random(),
        left: Math.random() * 98,
        emoji: BG_EMOJIS[Math.floor(Math.random() * BG_EMOJIS.length)],
        dur: 5 + Math.random() * 6,
        size: 13 + Math.random() * 20,
      }]);
    }, 650);
    return () => clearInterval(id);
  }, []);
  return <>
    {items.map(item => (
      <span key={item.id} className="floating-bg"
        style={{ left: `${item.left}%`, bottom: 0, fontSize: item.size, animationDuration: `${item.dur}s` }}>
        {item.emoji}
      </span>
    ))}
  </>;
}

/* ═══════════════════════════════════════════════════════════
   BACKGROUND BLOBS
═══════════════════════════════════════════════════════════ */
function Blobs() {
  const blobs = [
    { color: 'rgba(255,182,193,0.38)', top: '-90px', left: '-90px', size: 420, delay: '0s', dur: '9s' },
    { color: 'rgba(200,182,226,0.32)', bottom: '-90px', right: '-90px', size: 380, delay: '3s', dur: '11s' },
    { color: 'rgba(255,203,164,0.26)', top: '42%', left: '58%', size: 300, delay: '6s', dur: '13s' },
    { color: 'rgba(255,182,193,0.22)', top: '28%', left: '-70px', size: 220, delay: '1.5s', dur: '8s' },
  ];
  return (
    <div style={{ position: 'fixed', inset: 0, overflow: 'hidden', pointerEvents: 'none', zIndex: 0 }}>
      {blobs.map((b, i) => (
        <div key={i} style={{
          position: 'absolute', top: b.top, left: b.left, right: b.right, bottom: b.bottom,
          width: b.size, height: b.size,
          background: b.color, filter: 'blur(52px)',
          animation: `blobMorph ${b.dur} ease-in-out infinite`,
          animationDelay: b.delay,
        }} />
      ))}
    </div>
  );
}

/* ═══════════════════════════════════════════════════════════
   HERO SECTION
═══════════════════════════════════════════════════════════ */
function Hero() {
  const [hugCount, setHugCount] = useState(0);
  const [bursts, setBursts] = useState([]);

  const sendHug = () => {
    setHugCount(h => h + 1);
    setBursts(p => [...p, ...Array.from({ length: 8 }, (_, i) => ({
      id: Date.now() + i,
      x: 20 + Math.random() * 60,
      emoji: ['💗','💖','💕','💞','🌸','✨'][Math.floor(Math.random() * 6)],
      dur: 1.4 + Math.random() * 1.2,
    }))]);
  };

  const msg = hugCount === 0 ? '' :
    hugCount >= 20 ? '🥺 babe... she is so so loved' :
    hugCount >= 12 ? '💞 the AMOUNT of hugs she is getting right now' :
    hugCount >= 6 ? '💕 she is getting ALL the hugs!!' :
    '🤍 hug delivered straight to her heart!';

  const deco = [
    { e:'🌸', top:'11%', left:'5%', del:'0s', dur:'3.3s', sz:30 },
    { e:'✨', top:'16%', right:'7%', del:'0.8s', dur:'4.1s', sz:26 },
    { e:'💫', bottom:'26%', left:'8%', del:'0.4s', dur:'3.8s', sz:24 },
    { e:'🌟', bottom:'32%', right:'5%', del:'1.6s', dur:'5.1s', sz:32 },
    { e:'💗', top:'68%', left:'18%', del:'1s', dur:'3.5s', sz:22 },
    { e:'🫧', top:'38%', right:'11%', del:'2s', dur:'4.3s', sz:28 },
  ];

  return (
    <section style={{
      minHeight: '100vh', display: 'flex', flexDirection: 'column',
      alignItems: 'center', justifyContent: 'center',
      textAlign: 'center', padding: '60px 24px',
      position: 'relative', zIndex: 1,
    }}>
      {bursts.map(b => (
        <span key={b.id} style={{
          position: 'fixed', left: `${b.x}%`, top: '50%',
          fontSize: 22, animation: `floatUp ${b.dur}s ease-out forwards`,
          pointerEvents: 'none', zIndex: 10,
        }}>{b.emoji}</span>
      ))}
      {deco.map((d, i) => (
        <span key={i} style={{
          position: 'absolute', top: d.top, left: d.left, right: d.right, bottom: d.bottom,
          fontSize: d.sz, animation: `floatBob ${d.dur} ease-in-out infinite`,
          animationDelay: d.del, opacity: 0.55, pointerEvents: 'none',
        }}>{d.e}</span>
      ))}

      <div style={{ fontSize: 58, animation: 'wiggle 1.1s ease-in-out infinite', marginBottom: 22 }}>🚨</div>

      <h1 className="shimmer-text" style={{
        fontFamily: "'Pacifico', cursive",
        fontSize: 'clamp(28px, 7vw, 70px)',
        lineHeight: 1.22, marginBottom: 20,
      }}>
        Emergency Period<br />Care Center 💗
      </h1>

      <div className="glass" style={{ padding: '16px 30px', borderRadius: 44, maxWidth: 580, marginBottom: 14 }}>
        <p style={{
          fontFamily: "'Nunito', sans-serif",
          fontSize: 'clamp(13px, 2.6vw, 18px)',
          color: '#8B5A72', lineHeight: 1.7, fontWeight: 700,
        }}>
          Side effects may include: mood swings, stealing hoodies, demanding snacks at
          midnight, and occasional glaring at the ceiling for no reason. 🌸
        </p>
      </div>

      <p style={{
        fontFamily: "'Nunito', sans-serif", fontSize: 'clamp(12px, 2vw, 15px)',
        color: '#C4A0B0', fontStyle: 'italic', marginBottom: 38, fontWeight: 600,
      }}>
        ✨ You are legally entitled to unlimited cuddles this entire week ✨
      </p>

      <button className="hug-btn glass glow-pink" onClick={sendHug} style={{
        padding: '17px 46px', borderRadius: 54,
        border: '2px solid rgba(255,107,157,0.48)',
        background: 'linear-gradient(135deg, rgba(255,182,193,0.5), rgba(200,182,226,0.5))',
        fontFamily: "'Nunito', sans-serif",
        fontSize: 'clamp(15px, 2.6vw, 20px)',
        fontWeight: 900, color: '#E84D8A', letterSpacing: 0.5,
      }}>
        Send Unlimited Hugs 🤗{hugCount > 0 && ` (×${hugCount})`}
      </button>

      {msg && (
        <p style={{
          marginTop: 18, fontFamily: "'Nunito', sans-serif",
          fontSize: 15, color: '#C77DFF', fontWeight: 900,
          animation: 'popIn 0.4s cubic-bezier(.34,1.56,.64,1) both',
        }}>{msg}</p>
      )}

      <div style={{
        position: 'absolute', bottom: 30, left: '50%',
        transform: 'translateX(-50%)',
        animation: 'bounce 2.2s ease-in-out infinite',
        fontSize: 26, opacity: 0.38, cursor: 'default',
      }}>↓</div>
    </section>
  );
}

/* ═══════════════════════════════════════════════════════════
   MOOD METER
═══════════════════════════════════════════════════════════ */
const MOODS = {
  '😤 Angry': {
    msg: "Totally valid. Everything is annoying right now and you have every right to be mad at literally everything. Including the weather, spoons, and the concept of time.",
    action: "Prescription: My hoodie + screaming into a pillow + snacks.",
    color: '#FF6B6B',
  },
  '😭 Crying': {
    msg: "Cry it out, babe. Those tears are valid AND beautiful. I have tissues, I have arms, I have unlimited shoulder space. Come here.",
    action: "Prescription: One infinite shoulder to cry on. No appointment needed.",
    color: '#74B9FF',
  },
  '🍫 Needs Chocolate': {
    msg: "CHOCOLATE EMERGENCY ACTIVATED. Every Kit-Kat, Dairy Milk, and hot cocoa in the universe is being deployed to your location immediately. 🚀",
    action: "ETA: The literal second I can reach a store. Already putting shoes on.",
    color: '#8B6355',
  },
  '🥺 Wants Attention': {
    msg: "COME HERE RIGHT NOW. I am giving you ALL the attention. Every last bit. You have 100% of my focus and 0% is going anywhere else.",
    action: "Attention level: MAXIMUM. Other things: Cancelled.",
    color: '#A29BFE',
  },
  '⚔️ Will Fight Anyone': {
    msg: "SAY LESS. Give me names. I am your hype squad, your bodyguard, your getaway driver, and your alibi.",
    action: "Status: Threats assessed. Your honor: Fully defended. 💪",
    color: '#FD79A8',
  },
};

function MoodMeter() {
  const [mood, setMood] = useState(null);

  return (
    <section style={{ padding: '60px 24px', maxWidth: 800, margin: '0 auto', position: 'relative', zIndex: 1 }}>
      <h2 className="gradient-text" style={{
        fontFamily: "'Pacifico', cursive",
        fontSize: 'clamp(24px, 5vw, 46px)',
        textAlign: 'center', marginBottom: 10,
      }}>
        Current Mood Detector 📊
      </h2>
      <p style={{
        textAlign: 'center', fontFamily: "'Nunito', sans-serif",
        color: '#C4A0B0', marginBottom: 36, fontSize: 16, fontWeight: 700,
      }}>
        Select your vibe — I'll figure out exactly what you need 💗
      </p>
      <div style={{ display: 'flex', flexWrap: 'wrap', gap: 12, justifyContent: 'center', marginBottom: 28 }}>
        {Object.keys(MOODS).map(m => (
          <button key={m}
            className={`mood-btn glass${mood === m ? ' selected' : ''}`}
            onClick={() => setMood(m === mood ? null : m)}
            style={{
              padding: '13px 23px', borderRadius: 44,
              border: mood === m ? '2px solid #FF6B9D' : '1px solid rgba(255,255,255,0.58)',
              fontFamily: "'Nunito', sans-serif",
              fontSize: 'clamp(13px, 2.4vw, 16px)', fontWeight: 800,
              color: mood === m ? '#E84D8A' : '#9B6F7F',
              background: mood === m ? 'rgba(255,107,157,0.15)' : undefined,
            }}>{m}</button>
        ))}
      </div>
      {mood && (
        <div className="glass" style={{
          padding: '28px 30px', borderRadius: 26,
          textAlign: 'center',
          animation: 'popIn 0.4s cubic-bezier(.34,1.56,.64,1) both',
          border: `2px solid ${MOODS[mood].color}55`,
        }}>
          <p style={{
            fontFamily: "'Nunito', sans-serif",
            fontSize: 'clamp(14px, 2.5vw, 18px)',
            color: '#5D3A4A', fontWeight: 700,
            lineHeight: 1.65, marginBottom: 18,
          }}>{MOODS[mood].msg}</p>
          <span style={{
            display: 'inline-block', fontFamily: "'Nunito', sans-serif",
            fontSize: 14, fontWeight: 800, color: MOODS[mood].color,
            background: `${MOODS[mood].color}22`,
            padding: '11px 24px', borderRadius: 32,
          }}>{MOODS[mood].action}</span>
        </div>
      )}
    </section>
  );
}

/* ═══════════════════════════════════════════════════════════
   SURVIVAL KIT
═══════════════════════════════════════════════════════════ */
const KIT = [
  { emoji:'🍫', title:'Chocolate', desc:'The official medicine of period week. Not optional. Scientifically proven to fix 90% of all known problems.', color:'#8B6355' },
  { emoji:'🔥', title:'Heating Pad', desc:'Warm, cozy, and absolutely non-judgmental. Honestly better than most people.', color:'#FF7043' },
  { emoji:'🍦', title:'Ice Cream', desc:'Any flavor. Every flavor. All the scoops. There are officially zero rules this week.', color:'#E91E8C' },
  { emoji:'🤗', title:'Cuddles', desc:'Prescription: Unlimited. Available 24/7. Delivery time: zero actual seconds.', color:'#FF6B9D' },
  { emoji:'😂', title:'Memes', desc:'Curated specifically for you. Guaranteed to produce at least one beautiful ugly laugh.', color:'#C77DFF' },
  { emoji:'💖', title:'Extra Love', desc:'Delivered directly to your heart. Quantity: more than you think you deserve (which means a lot, because you deserve a lot).', color:'#E84D8A' },
];

function SurvivalKit() {
  const [hov, setHov] = useState(null);
  return (
    <section style={{ padding: '60px 24px', maxWidth: 980, margin: '0 auto', position: 'relative', zIndex: 1 }}>
      <h2 className="gradient-text" style={{
        fontFamily: "'Pacifico', cursive",
        fontSize: 'clamp(24px, 5vw, 46px)',
        textAlign: 'center', marginBottom: 10,
      }}>Period Survival Kit 🎁</h2>
      <p style={{
        textAlign: 'center', fontFamily: "'Nunito', sans-serif",
        color: '#C4A0B0', marginBottom: 36, fontSize: 16, fontWeight: 700,
      }}>Everything you need — curated with love 💕</p>
      <div className="card-grid-3">
        {KIT.map((item, i) => (
          <div key={i} className="glass lift"
            onMouseEnter={() => setHov(i)} onMouseLeave={() => setHov(null)}
            style={{
              padding: '30px 22px', borderRadius: 26, textAlign: 'center',
              background: hov === i ? `${item.color}1A` : 'rgba(255,255,255,0.28)',
              border: hov === i ? `1.5px solid ${item.color}66` : '1px solid rgba(255,255,255,0.58)',
              transition: 'background 0.3s, border 0.3s',
            }}>
            <div style={{
              fontSize: 52, marginBottom: 13, display: 'inline-block',
              animation: hov === i ? 'wiggle 0.35s ease-in-out infinite' : `floatBob ${3 + i * 0.35}s ease-in-out infinite`,
              animationDelay: `${i * 0.22}s`,
            }}>{item.emoji}</div>
            <h3 style={{ fontFamily:"'Nunito', sans-serif", fontWeight:900, fontSize:19, color:item.color, marginBottom:9 }}>
              {item.title}
            </h3>
            <p style={{ fontFamily:"'Nunito', sans-serif", fontSize:13, color:'#9B6F7F', lineHeight:1.7 }}>
              {item.desc}
            </p>
          </div>
        ))}
      </div>
    </section>
  );
}

/* ═══════════════════════════════════════════════════════════
   THINGS I LOVE ABOUT YOU
═══════════════════════════════════════════════════════════ */
const LOVES = [
  { text:"Even your angry texts are cute.", emoji:"💌", color:'#FFB3C6' },
  { text:"You deserve the world and unlimited snacks.", emoji:"🌍", color:'#C8B6E2' },
  { text:"You are stronger than your cramps.", emoji:"💪", color:'#FFCBA4' },
  { text:"I'd still choose you during mood swings.", emoji:"🔄", color:'#B8E6B8' },
  { text:"Your face makes my whole day better.", emoji:"☀️", color:'#FFE4B5' },
  { text:"You're beautiful even when you don't feel it.", emoji:"✨", color:'#E4B5FF' },
  { text:"I love you at 100% and also at 12%.", emoji:"🔋", color:'#B5D5FF' },
  { text:"You're my favorite person. Just a fact.", emoji:"📌", color:'#FFB5D5' },
];

function LoveSection() {
  const [flipped, setFlipped] = useState({});
  return (
    <section style={{ padding: '60px 24px', maxWidth: 980, margin: '0 auto', position: 'relative', zIndex: 1 }}>
      <h2 className="gradient-text" style={{
        fontFamily: "'Pacifico', cursive",
        fontSize: 'clamp(24px, 5vw, 46px)',
        textAlign: 'center', marginBottom: 10,
      }}>Things I Love About You 💝</h2>
      <p style={{
        textAlign: 'center', fontFamily: "'Nunito', sans-serif",
        color: '#C4A0B0', marginBottom: 36, fontSize: 16, fontWeight: 700,
      }}>Tap a card to reveal a love note 🌸</p>
      <div className="card-grid-4">
        {LOVES.map((love, i) => (
          <div key={i}
            onClick={() => setFlipped(p => ({ ...p, [i]: !p[i] }))}
            style={{ perspective: 1000, height: 164, cursor: 'pointer' }}>
            <div style={{
              width:'100%', height:'100%', position:'relative',
              transformStyle:'preserve-3d',
              transition:'transform 0.68s cubic-bezier(.4,0,.2,1)',
              transform: flipped[i] ? 'rotateY(180deg)' : 'rotateY(0deg)',
            }}>
              {/* Front */}
              <div style={{
                position:'absolute', inset:0, borderRadius:22,
                backfaceVisibility:'hidden',
                display:'flex', flexDirection:'column', alignItems:'center', justifyContent:'center',
                background: `${love.color}44`,
                border: `1px solid ${love.color}88`,
                backdropFilter:'blur(12px)', WebkitBackdropFilter:'blur(12px)',
              }}>
                <span style={{ fontSize:44, marginBottom:9 }}>{love.emoji}</span>
                <span style={{ fontFamily:"'Nunito', sans-serif", fontSize:12, color:'#9B6F7F', fontWeight:700 }}>
                  Tap to reveal 💌
                </span>
              </div>
              {/* Back */}
              <div style={{
                position:'absolute', inset:0, borderRadius:22,
                backfaceVisibility:'hidden',
                transform:'rotateY(180deg)',
                display:'flex', alignItems:'center', justifyContent:'center',
                padding:20, textAlign:'center',
                background: `linear-gradient(135deg, ${love.color}66, ${love.color}33)`,
                border: `1px solid ${love.color}88`,
              }}>
                <p style={{
                  fontFamily:"'Dancing Script', cursive",
                  fontSize:'clamp(15px, 2.6vw, 19px)',
                  color:'#4D2A3A', fontWeight:700, lineHeight:1.42, margin:0,
                }}>{love.text}</p>
              </div>
            </div>
          </div>
        ))}
      </div>
    </section>
  );
}

/* ═══════════════════════════════════════════════════════════
   MINI GAME — HAPPINESS CATCHER
═══════════════════════════════════════════════════════════ */
const CANDIES = ['🍫','💗','🍦','🌸','⭐','🍓','🧁','🍭','💕','🎀'];

function MiniGame() {
  const [active, setActive] = useState(false);
  const [over, setOver] = useState(false);
  const [score, setScore] = useState(0);
  const [missed, setMissed] = useState(0);
  const [happy, setHappy] = useState(0);
  const [items, setItems] = useState([]);
  const activeRef = useRef(false);
  const idRef = useRef(0);

  const start = () => {
    setScore(0); setMissed(0); setHappy(0); setItems([]); setOver(false);
    setActive(true); activeRef.current = true;
  };

  useEffect(() => {
    if (!active) return;
    const iv = setInterval(() => {
      if (!activeRef.current) return;
      const item = {
        id: idRef.current++,
        emoji: CANDIES[Math.floor(Math.random() * CANDIES.length)],
        left: 6 + Math.random() * 84,
        dur: 2.4 + Math.random() * 2.2,
        caught: false,
      };
      setItems(p => [...p, item]);
      setTimeout(() => {
        setItems(p => {
          const found = p.find(i => i.id === item.id);
          if (found && !found.caught) {
            setMissed(m => {
              const nm = m + 1;
              if (nm >= 5) { setOver(true); setActive(false); activeRef.current = false; }
              return nm;
            });
          }
          return p.filter(i => i.id !== item.id);
        });
      }, (item.dur + 0.25) * 1000);
    }, 1050);
    return () => { clearInterval(iv); activeRef.current = false; };
  }, [active]);

  const catchIt = (id) => {
    setItems(p => p.map(i => i.id === id ? { ...i, caught: true } : i));
    setTimeout(() => setItems(p => p.filter(i => i.id !== id)), 160);
    setScore(s => s + 1);
    setHappy(h => Math.min(100, h + 10));
  };

  const happyLabel = happy >= 90 ? '🥰 MAX HAPPINESS!!' : happy >= 70 ? '😍 So happy~' : happy >= 40 ? '😊 Getting there!' : happy >= 10 ? '🙂 A little happy~' : '😶 Catch the goodies!';

  return (
    <section style={{ padding: '60px 24px', maxWidth: 680, margin: '0 auto', position: 'relative', zIndex: 1 }}>
      <h2 className="gradient-text" style={{
        fontFamily: "'Pacifico', cursive",
        fontSize: 'clamp(24px, 5vw, 46px)',
        textAlign: 'center', marginBottom: 10,
      }}>Happiness Catcher 🎮</h2>
      <p style={{
        textAlign:'center', fontFamily:"'Nunito', sans-serif",
        color:'#C4A0B0', marginBottom:22, fontSize:16, fontWeight:700,
      }}>Catch falling goodies to fill your happiness meter 💕</p>

      <div style={{ display:'flex', gap:14, justifyContent:'center', marginBottom:16, flexWrap:'wrap' }}>
        {[['Caught ✅', score, '#FF6B9D'], ['Missed ❌', `${missed}/5`, '#C77DFF']].map(([label, val, color]) => (
          <div key={label} className="glass" style={{ padding:'10px 26px', borderRadius:22, textAlign:'center', minWidth:95 }}>
            <div style={{ fontFamily:"'Nunito', sans-serif", fontWeight:900, fontSize:23, color }}>{val}</div>
            <div style={{ fontFamily:"'Nunito', sans-serif", fontSize:11, color:'#C4A0B0', fontWeight:700 }}>{label}</div>
          </div>
        ))}
      </div>

      <div style={{ marginBottom:16, padding:'0 2px' }}>
        <div style={{ display:'flex', justifyContent:'space-between', marginBottom:7 }}>
          <span style={{ fontFamily:"'Nunito', sans-serif", fontSize:13, color:'#9B6F7F', fontWeight:700 }}>Happiness Meter 💖</span>
          <span style={{ fontFamily:"'Nunito', sans-serif", fontSize:13, color:'#FF6B9D', fontWeight:900 }}>{happy}% — {happyLabel}</span>
        </div>
        <div style={{ background:'rgba(255,255,255,0.38)', borderRadius:22, height:20, overflow:'hidden', boxShadow:'inset 0 2px 8px rgba(0,0,0,0.07)' }}>
          <div style={{
            height:'100%', width:`${happy}%`,
            background:'linear-gradient(90deg, #FF6B9D, #C77DFF, #FF9E7E)',
            borderRadius:22,
            transition:'width 0.55s cubic-bezier(.34,1.56,.64,1)',
            boxShadow:'0 0 14px rgba(255,107,157,0.55)',
          }} />
        </div>
      </div>

      <div className="glass" style={{ position:'relative', height:320, borderRadius:26, overflow:'hidden', background:'rgba(255,255,255,0.14)', marginBottom:18 }}>
        {!active && !over && (
          <div style={{ position:'absolute', inset:0, display:'flex', flexDirection:'column', alignItems:'center', justifyContent:'center', gap:16 }}>
            <div style={{ fontSize:54, animation:'bounce 1.3s ease-in-out infinite' }}>🍫</div>
            <p style={{ fontFamily:"'Nunito', sans-serif", color:'#9B6F7F', fontWeight:700, fontSize:15, textAlign:'center', padding:'0 24px' }}>
              Tap the falling treats before they disappear!<br/>
              <span style={{ fontSize:13, color:'#C4A0B0' }}>Miss 5 = game over (real chocolate still coming, don't worry)</span>
            </p>
            <button onClick={start} style={{
              padding:'14px 36px', borderRadius:44, border:'none',
              background:'linear-gradient(135deg, #FF6B9D, #C77DFF)',
              color:'white', fontFamily:"'Nunito', sans-serif",
              fontWeight:900, fontSize:17, cursor:'pointer',
              boxShadow:'0 8px 28px rgba(255,107,157,0.4)',
              transition:'transform 0.2s',
            }}>Start Game! 🎮</button>
          </div>
        )}
        {over && (
          <div style={{
            position:'absolute', inset:0, display:'flex', flexDirection:'column',
            alignItems:'center', justifyContent:'center', gap:13,
            background:'rgba(255,255,255,0.38)', backdropFilter:'blur(14px)',
            WebkitBackdropFilter:'blur(14px)', animation:'popIn 0.4s both',
          }}>
            <div style={{ fontSize:52 }}>😅</div>
            <p style={{ fontFamily:"'Nunito', sans-serif", color:'#FF6B9D', fontWeight:900, fontSize:24 }}>Game Over!</p>
            <p style={{ fontFamily:"'Nunito', sans-serif", color:'#9B6F7F', fontSize:14, textAlign:'center', padding:'0 24px' }}>
              You caught {score} treats! Honestly, the real happiness is you. 🍫
            </p>
            <button onClick={start} style={{
              padding:'12px 30px', borderRadius:44, border:'none',
              background:'linear-gradient(135deg, #FF6B9D, #C77DFF)',
              color:'white', fontFamily:"'Nunito', sans-serif",
              fontWeight:800, fontSize:15, cursor:'pointer',
            }}>Try Again 💕</button>
          </div>
        )}
        {items.map(item => (
          <span key={item.id} className="falling-candy"
            onClick={() => !item.caught && catchIt(item.id)}
            style={{
              left:`${item.left}%`, top:0,
              animationDuration:`${item.dur}s`,
              opacity: item.caught ? 0 : 1,
              transform: item.caught ? 'scale(0)' : undefined,
            }}>{item.emoji}</span>
        ))}
      </div>
    </section>
  );
}

/* ═══════════════════════════════════════════════════════════
   PLAYLIST
═══════════════════════════════════════════════════════════ */
const SONGS = [
  { title:'Golden Hour', artist:'JVKE', emoji:'🌅', vibe:'warm & golden', color:'#FFE4B5' },
  { title:'Until I Found You', artist:'Stephen Sanchez', emoji:'💌', vibe:'pure romance', color:'#FFB3C6' },
  { title:'Lover', artist:'Taylor Swift', emoji:'💚', vibe:'soft & dreamy', color:'#B8E6B8' },
  { title:'Die For You', artist:'The Weeknd', emoji:'🔥', vibe:'intense love', color:'#FFCBA4' },
  { title:'Comfortable', artist:'H.E.R.', emoji:'☁️', vibe:'cozy vibes only', color:'#C8B6E2' },
  { title:'Peaches', artist:'Justin Bieber', emoji:'🍑', vibe:'feel-good warm', color:'#FFB5D5' },
];

function Playlist() {
  const [playing, setPlaying] = useState(null);
  return (
    <section style={{ padding:'60px 24px', maxWidth:680, margin:'0 auto', position:'relative', zIndex:1 }}>
      <h2 className="gradient-text" style={{
        fontFamily:"'Pacifico', cursive",
        fontSize:'clamp(24px, 5vw, 46px)',
        textAlign:'center', marginBottom:10,
      }}>Comfort Playlist 🎵</h2>
      <p style={{ textAlign:'center', fontFamily:"'Nunito', sans-serif", color:'#C4A0B0', marginBottom:36, fontSize:16, fontWeight:700 }}>
        Songs handpicked just for you — with lots of love 🎶
      </p>
      <div style={{ display:'flex', flexDirection:'column', gap:12 }}>
        {SONGS.map((song, i) => (
          <div key={i} className="glass lift"
            onClick={() => setPlaying(playing === i ? null : i)}
            style={{
              padding:'14px 18px', borderRadius:20,
              display:'flex', alignItems:'center', gap:14,
              background: playing === i ? `${song.color}55` : 'rgba(255,255,255,0.28)',
              border: playing === i ? `1.5px solid ${song.color}` : '1px solid rgba(255,255,255,0.58)',
              transition:'background 0.3s, border 0.3s',
            }}>
            <div style={{
              width:50, height:50, borderRadius:14, flexShrink:0,
              background:`linear-gradient(135deg, ${song.color}, ${song.color}88)`,
              display:'flex', alignItems:'center', justifyContent:'center',
              fontSize:26,
              animation: playing === i ? 'pulse 1.3s ease-in-out infinite' : 'none',
              boxShadow: playing === i ? `0 0 20px ${song.color}` : 'none',
              transition:'box-shadow 0.3s',
            }}>{song.emoji}</div>
            <div style={{ flex:1 }}>
              <div style={{ fontFamily:"'Nunito', sans-serif", fontWeight:900, fontSize:15, color:'#4D2A3A' }}>{song.title}</div>
              <div style={{ fontFamily:"'Nunito', sans-serif", fontSize:13, color:'#C4A0B0', marginTop:2 }}>
                {song.artist} · {song.vibe}
              </div>
            </div>
            <div style={{
              width:36, height:36, borderRadius:'50%', flexShrink:0, fontSize:13,
              background: playing === i ? 'linear-gradient(135deg, #FF6B9D, #C77DFF)' : 'rgba(255,107,157,0.16)',
              display:'flex', alignItems:'center', justifyContent:'center',
              transition:'all 0.3s',
              boxShadow: playing === i ? '0 0 14px rgba(255,107,157,0.55)' : 'none',
              color: playing === i ? 'white' : '#FF6B9D',
            }}>{playing === i ? '⏸' : '▶'}</div>
          </div>
        ))}
      </div>
      {playing !== null && (
        <p style={{ textAlign:'center', fontFamily:"'Nunito', sans-serif", fontSize:13, color:'#C77DFF', marginTop:16, fontStyle:'italic', fontWeight:700 }}>
          🎵 Now vibing: {SONGS[playing].title} — find it on Spotify! 🎵
        </p>
      )}
    </section>
  );
}

/* ═══════════════════════════════════════════════════════════
   SNACK COUNTDOWN
═══════════════════════════════════════════════════════════ */
function Countdown() {
  const [t, setT] = useState({ h:0, m:23, s:17 });
  useEffect(() => {
    const iv = setInterval(() => {
      setT(p => {
        let { h, m, s } = p;
        s--;
        if (s < 0) { s = 59; m--; }
        if (m < 0) { m = 59; h--; }
        if (h < 0) return { h:0, m:30, s:0 };
        return { h, m, s };
      });
    }, 1000);
    return () => clearInterval(iv);
  }, []);
  const pad = n => String(n).padStart(2, '0');

  return (
    <section style={{ padding:'60px 24px', maxWidth:600, margin:'0 auto', textAlign:'center', position:'relative', zIndex:1 }}>
      <h2 className="gradient-text" style={{ fontFamily:"'Pacifico', cursive", fontSize:'clamp(24px, 5vw, 46px)', marginBottom:10 }}>
        Snack O'Clock ⏰
      </h2>
      <p style={{ fontFamily:"'Nunito', sans-serif", color:'#C4A0B0', marginBottom:32, fontSize:16, fontWeight:700 }}>
        Next snack delivery authorized in:
      </p>
      <div style={{ display:'flex', gap:18, justifyContent:'center', flexWrap:'wrap' }}>
        {[['Hours', t.h], ['Minutes', t.m], ['Seconds', t.s]].map(([label, val]) => (
          <div key={label} className="glass glow-pink" style={{ padding:'24px 28px', borderRadius:24, minWidth:96 }}>
            <div style={{ fontFamily:"'Pacifico', cursive", fontSize:48, color:'#FF6B9D', lineHeight:1 }}>{pad(val)}</div>
            <div style={{ fontFamily:"'Nunito', sans-serif", fontSize:11, color:'#C4A0B0', fontWeight:800, textTransform:'uppercase', letterSpacing:1.6, marginTop:6 }}>{label}</div>
          </div>
        ))}
      </div>
      <p style={{ fontFamily:"'Nunito', sans-serif", fontSize:13, color:'#C77DFF', marginTop:22, fontStyle:'italic', fontWeight:700 }}>
        (or honestly just ask right now — zero rules this week 🍫✨)
      </p>
    </section>
  );
}

/* ═══════════════════════════════════════════════════════════
   LOVE LETTER
═══════════════════════════════════════════════════════════ */
function LoveLetter() {
  return (
    <section style={{ padding:'60px 24px 110px', maxWidth:700, margin:'0 auto', textAlign:'center', position:'relative', zIndex:1 }}>
      <h2 className="gradient-text" style={{ fontFamily:"'Pacifico', cursive", fontSize:'clamp(24px, 5vw, 46px)', marginBottom:38 }}>
        A Note From Your Person 💌
      </h2>
      <div className="glass-pink" style={{ padding:'42px 34px', borderRadius:30 }}>
        <div style={{ fontSize:48, marginBottom:26, animation:'heartbeat 2s ease-in-out infinite' }}>💗</div>
        <p style={{
          fontFamily:"'Dancing Script', cursive",
          fontSize:'clamp(18px, 3.8vw, 25px)',
          color:'#4D2A3A', lineHeight:1.9, fontWeight:600, marginBottom:22,
        }}>
          I know periods can be exhausting and painful sometimes, but I just want you to know
          I'm always here for you — for the cramps, mood swings, random cravings,
          and everything in between.
        </p>
        <p style={{
          fontFamily:"'Dancing Script', cursive",
          fontSize:'clamp(18px, 3.8vw, 25px)',
          color:'#4D2A3A', lineHeight:1.9, fontWeight:600, marginBottom:30,
        }}>
          You deserve comfort, rest, love, and all the chocolates in the world.
          I'm so lucky to be your person. Rest well, my beautiful girl. 💗
        </p>
        <div style={{
          fontFamily:"'Dancing Script', cursive",
          fontSize:23, color:'#FF6B9D', fontWeight:700,
          borderTop:'1px solid rgba(255,107,157,0.28)', paddingTop:22,
        }}>
          — Always by your side, every single day 🌸
        </div>
      </div>
      <div style={{ marginTop:44, fontSize:30, animation:'heartbeat 2.5s ease-in-out infinite', letterSpacing:8 }}>
        💗 💕 💖 💗 💕 💖
      </div>
      <p style={{ fontFamily:"'Nunito', sans-serif", fontSize:12, color:'#C4A0B0', marginTop:22, fontWeight:700 }}>
        Made with 💗 and way too much love · Certified Boyfriend-Made™
      </p>
    </section>
  );
}

/* ═══════════════════════════════════════════════════════════
   APP ROOT
═══════════════════════════════════════════════════════════ */
export default function App() {
  return (
    <div style={{
      minHeight:'100vh',
      background:'linear-gradient(135deg, #FFF0F5 0%, #F5F0FF 25%, #FFF5F0 50%, #F0F8FF 75%, #FFF0F5 100%)',
      backgroundSize:'400% 400%',
      animation:'gradientShift 18s ease infinite',
      overflowX:'hidden',
    }}>
      <style>{CSS}</style>
      <Blobs />
      <FloatingBg />
      <div style={{ position:'relative', zIndex:1 }}>
        <Hero />
        <div className="section-divider">🌸 ✨ 💗 ✨ 🌸</div>
        <MoodMeter />
        <div className="section-divider">🌸 ✨ 💗 ✨ 🌸</div>
        <SurvivalKit />
        <div className="section-divider">🌸 ✨ 💗 ✨ 🌸</div>
        <LoveSection />
        <div className="section-divider">🌸 ✨ 💗 ✨ 🌸</div>
        <MiniGame />
        <div className="section-divider">🌸 ✨ 💗 ✨ 🌸</div>
        <Playlist />
        <div className="section-divider">🌸 ✨ 💗 ✨ 🌸</div>
        <Countdown />
        <div className="section-divider">🌸 ✨ 💗 ✨ 🌸</div>
        <LoveLetter />
      </div>
    </div>
  );
}
