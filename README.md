import React, { useEffect, useMemo, useState } from "react";
import { motion, AnimatePresence } from "framer-motion";
import { Heart, Gift, Cake, Sparkles, Camera, Music, Star, Lock, PartyPopper } from "lucide-react";

const photos = [
  {
    title: "The cartoon version of us",
    note: "The cutest sticker-style memory, because even distance looks softer like this.",
    src: "data:image/jpeg;base64,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",
  },
  {
    title: "Our first proper birthday-worthy memory",
    note: "This one feels like the kind of photo I will always keep close.",
    src: "data:image/jpeg;base64,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",
  }
];

const reasons = [
  "Your smile makes normal days feel special.",
  "You make me feel loved even from far away.",
  "You are cute, dramatic, sweet, and completely mine.",
  "I love how your little things stay in my heart.",
  "Even distance cannot make you feel less close to me.",
  "You are my favorite person to annoy, love, and protect.",
];

const promises = [
  "I will keep choosing you, even on the difficult days.",
  "I will make more memories with you, not only online but beside you.",
  "I will celebrate you properly one day, with your real cake in front of us.",
  "Until then, I will keep loving you from every distance.",
];

function FloatingHearts() {
  const hearts = useMemo(
    () =>
      Array.from({ length: 22 }, (_, i) => ({
        id: i,
        left: Math.random() * 100,
        delay: Math.random() * 8,
        duration: 8 + Math.random() * 9,
        size: 12 + Math.random() * 18,
      })),
    []
  );

  return (
    <div className="pointer-events-none fixed inset-0 overflow-hidden">
      {hearts.map((heart) => (
        <motion.div
          key={heart.id}
          className="absolute text-pink-300/70"
          style={{ left: `${heart.left}%`, bottom: -40, fontSize: heart.size }}
          animate={{ y: [0, -900], x: [0, Math.random() * 80 - 40], opacity: [0, 1, 0] }}
          transition={{ duration: heart.duration, repeat: Infinity, delay: heart.delay, ease: "easeInOut" }}
        >
          ❤
        </motion.div>
      ))}
    </div>
  );
}

function SparkleField() {
  const sparkles = useMemo(
    () =>
      Array.from({ length: 60 }, (_, i) => ({
        id: i,
        top: Math.random() * 100,
        left: Math.random() * 100,
        delay: Math.random() * 3,
        scale: 0.4 + Math.random() * 0.9,
      })),
    []
  );

  return (
    <div className="pointer-events-none fixed inset-0 overflow-hidden">
      {sparkles.map((s) => (
        <motion.span
          key={s.id}
          className="absolute h-1.5 w-1.5 rounded-full bg-white shadow-[0_0_16px_rgba(255,255,255,0.95)]"
          style={{ top: `${s.top}%`, left: `${s.left}%` }}
          animate={{ opacity: [0.15, 1, 0.15], scale: [s.scale, s.scale * 1.9, s.scale] }}
          transition={{ repeat: Infinity, duration: 2.2, delay: s.delay }}
        />
      ))}
    </div>
  );
}

function Section({ children, className = "" }) {
  return (
    <motion.section
      initial={{ opacity: 0, y: 45 }}
      whileInView={{ opacity: 1, y: 0 }}
      viewport={{ once: true, margin: "-100px" }}
      transition={{ duration: 0.8, ease: "easeOut" }}
      className={`mx-auto w-full max-w-6xl px-5 py-16 sm:px-8 ${className}`}
    >
      {children}
    </motion.section>
  );
}

function GlassCard({ children, className = "" }) {
  return <div className={`rounded-[2rem] border border-white/40 bg-white/25 p-6 shadow-2xl shadow-pink-900/10 backdrop-blur-xl ${className}`}>{children}</div>;
}

function BirthdayCake() {
  const [cut, setCut] = useState(false);

  return (
    <div className="flex flex-col items-center text-center">
      <motion.div
        animate={{ rotate: cut ? [0, -2, 2, -2, 0] : 0, scale: cut ? [1, 1.06, 1] : 1 }}
        transition={{ duration: 0.8 }}
        className="relative mb-7"
      >
        <motion.div
          animate={{ y: [0, -7, 0] }}
          transition={{ repeat: Infinity, duration: 2.4, ease: "easeInOut" }}
          className="text-[9rem] sm:text-[12rem] drop-shadow-2xl"
        >
          🎂
        </motion.div>
        <AnimatePresence>
          {cut && (
            <motion.div
              initial={{ opacity: 0, scale: 0.5, y: 20 }}
              animate={{ opacity: 1, scale: 1, y: 0 }}
              exit={{ opacity: 0 }}
              className="absolute -right-10 top-5 rounded-full bg-white/80 px-4 py-2 text-sm font-bold text-pink-600 shadow-xl"
            >
              First bite is for you 💗
            </motion.div>
          )}
        </AnimatePresence>
      </motion.div>

      <button
        onClick={() => setCut(true)}
        className="rounded-full bg-gradient-to-r from-pink-500 via-fuchsia-500 to-rose-500 px-8 py-4 text-lg font-bold text-white shadow-xl shadow-pink-500/30 transition hover:scale-105 active:scale-95"
      >
        Make a wish & cut the cake
      </button>

      {cut && (
        <motion.p initial={{ opacity: 0 }} animate={{ opacity: 1 }} className="mt-6 max-w-xl text-lg text-rose-700">
          I know I cannot be beside you tonight, so I brought your cake here. I cut it from my side, but in my heart, this is our cake.
        </motion.p>
      )}
    </div>
  );
}

export default function MistyBirthdaySurprise() {
  const [opened, setOpened] = useState(false);
  const [musicHint, setMusicHint] = useState(false);

  useEffect(() => {
    const timer = setTimeout(() => setMusicHint(true), 1800);
    return () => clearTimeout(timer);
  }, []);

  return (
    <main className="relative min-h-screen overflow-hidden bg-gradient-to-br from-rose-100 via-pink-100 to-fuchsia-200 text-rose-950">
      <div className="fixed inset-0 bg-[radial-gradient(circle_at_20%_20%,rgba(255,255,255,0.9),transparent_30%),radial-gradient(circle_at_80%_10%,rgba(255,192,203,0.7),transparent_28%),radial-gradient(circle_at_50%_90%,rgba(217,70,239,0.22),transparent_35%)]" />
      <SparkleField />
      <FloatingHearts />

      <div className="relative z-10">
        <section className="flex min-h-screen items-center justify-center px-5 py-12 text-center">
          <motion.div initial={{ opacity: 0, scale: 0.92 }} animate={{ opacity: 1, scale: 1 }} transition={{ duration: 0.9 }} className="w-full max-w-4xl">
            <div className="mb-6 flex justify-center">
              <motion.div animate={{ rotate: [0, 8, -8, 0] }} transition={{ repeat: Infinity, duration: 2.5 }} className="rounded-full bg-white/40 p-4 shadow-xl backdrop-blur-xl">
                <PartyPopper className="h-10 w-10 text-pink-600" />
              </motion.div>
            </div>

            <motion.p initial={{ opacity: 0, y: 15 }} animate={{ opacity: 1, y: 0 }} transition={{ delay: 0.25 }} className="mb-4 text-sm font-bold uppercase tracking-[0.35em] text-pink-600">
              A midnight surprise from far away
            </motion.p>

            <h1 className="text-5xl font-black leading-tight text-rose-700 drop-shadow-sm sm:text-7xl md:text-8xl">
              Happy Birthday,
              <span className="block bg-gradient-to-r from-pink-600 via-fuchsia-600 to-rose-500 bg-clip-text text-transparent">Misty</span>
            </h1>

            <p className="mx-auto mt-7 max-w-2xl text-lg leading-8 text-rose-800 sm:text-xl">
              I made this little world for you because distance should not stop your birthday from starting with love, sparkle, and me.
            </p>

            <div className="mt-10 flex flex-col items-center justify-center gap-4 sm:flex-row">
              <button
                onClick={() => setOpened(true)}
                className="group rounded-full bg-gradient-to-r from-pink-500 via-fuchsia-500 to-rose-500 px-9 py-4 text-lg font-bold text-white shadow-2xl shadow-pink-500/30 transition hover:scale-105 active:scale-95"
              >
                <span className="inline-flex items-center gap-2">
                  Open your surprise <Sparkles className="h-5 w-5 transition group-hover:rotate-12" />
                </span>
              </button>

              <a href="#cake" className="rounded-full border border-white/60 bg-white/35 px-9 py-4 text-lg font-bold text-rose-700 shadow-xl backdrop-blur-xl transition hover:scale-105">
                Go to cake moment
              </a>
            </div>

            <AnimatePresence>
              {musicHint && !opened && (
                <motion.div initial={{ opacity: 0, y: 12 }} animate={{ opacity: 1, y: 0 }} exit={{ opacity: 0 }} className="mx-auto mt-8 max-w-md rounded-3xl border border-white/50 bg-white/30 p-4 text-sm text-rose-700 shadow-lg backdrop-blur-xl">
                  Tip: play a soft birthday song on your phone before she opens this.
                </motion.div>
              )}
            </AnimatePresence>
          </motion.div>
        </section>

        <AnimatePresence>
          {opened && (
            <motion.div initial={{ opacity: 0 }} animate={{ opacity: 1 }} transition={{ duration: 0.8 }}>
              <Section>
                <GlassCard className="text-center">
                  <div className="mb-5 flex justify-center">
                    <Heart className="h-12 w-12 fill-pink-500 text-pink-500" />
                  </div>
                  <h2 className="text-4xl font-black text-rose-700 sm:text-5xl">My birthday wish for you</h2>
                  <p className="mx-auto mt-6 max-w-3xl text-lg leading-9 text-rose-800">
                    Happy birthday, my love. I know we are in different countries right now, but I still wanted you to feel that I am close to you tonight. You are special to me in a way I cannot fully explain. I hope this year gives you peace, happiness, success, and the kind of love that makes you feel safe every day.
                  </p>
                  <p className="mx-auto mt-5 max-w-3xl text-lg leading-9 text-rose-800">
                    I wish I could be beside you, hold your hand, see your smile, and cut your cake properly. Until that day comes, I will keep finding little ways to celebrate you from far away.
                  </p>
                </GlassCard>
              </Section>

              <Section>
                <div className="mb-9 text-center">
                  <div className="mb-4 flex justify-center"><Camera className="h-10 w-10 text-pink-600" /></div>
                  <h2 className="text-4xl font-black text-rose-700 sm:text-5xl">Little memories of us</h2>
                  <p className="mx-auto mt-4 max-w-2xl text-rose-700">These spaces are for our photos, screenshots, cute calls, and the moments I never want to forget.</p>
                </div>

                <div className="grid gap-6 sm:grid-cols-2">
                  {photos.map((photo, index) => (
                    <motion.div
                      key={photo.title}
                      initial={{ opacity: 0, y: 30 }}
                      whileInView={{ opacity: 1, y: 0 }}
                      viewport={{ once: true }}
                      transition={{ delay: index * 0.1 }}
                      className="group overflow-hidden rounded-[2rem] border border-white/50 bg-white/30 shadow-2xl shadow-pink-900/10 backdrop-blur-xl"
                    >
                      <div className="relative h-72 overflow-hidden">
                        <img src={photo.src} alt={photo.title} className="h-full w-full object-cover transition duration-700 group-hover:scale-110" />
                        <div className="absolute inset-0 bg-gradient-to-t from-rose-950/55 via-transparent to-transparent" />
                        <div className="absolute bottom-4 left-4 right-4 text-left text-white">
                          <p className="text-2xl font-black">{photo.title}</p>
                          <p className="mt-1 text-sm text-white/90">{photo.note}</p>
                        </div>
                      </div>
                    </motion.div>
                  ))}
                </div>
              </Section>

              <Section>
                <div className="mb-9 text-center">
                  <div className="mb-4 flex justify-center"><Star className="h-10 w-10 fill-yellow-300 text-yellow-400" /></div>
                  <h2 className="text-4xl font-black text-rose-700 sm:text-5xl">Reasons I love you</h2>
                </div>

                <div className="grid gap-5 md:grid-cols-3">
                  {reasons.map((reason, index) => (
                    <motion.div
                      key={reason}
                      whileHover={{ y: -8, rotate: index % 2 === 0 ? 1 : -1 }}
                      className="rounded-[2rem] border border-white/50 bg-white/35 p-6 shadow-xl shadow-pink-900/10 backdrop-blur-xl"
                    >
                      <div className="mb-4 flex h-12 w-12 items-center justify-center rounded-full bg-pink-500/15 text-xl font-black text-pink-600">{index + 1}</div>
                      <p className="text-lg font-semibold leading-8 text-rose-800">{reason}</p>
                    </motion.div>
                  ))}
                </div>
              </Section>

              <Section id="cake">
                <GlassCard>
                  <div className="mb-8 text-center">
                    <div className="mb-4 flex justify-center"><Cake className="h-11 w-11 text-pink-600" /></div>
                    <h2 className="text-4xl font-black text-rose-700 sm:text-5xl">Our midnight cake</h2>
                    <p className="mx-auto mt-4 max-w-2xl text-rose-700">When you reach this part, video call me. I have your cake ready on my side.</p>
                  </div>
                  <BirthdayCake />
                </GlassCard>
              </Section>

              <Section>
                <div className="grid gap-6 lg:grid-cols-[1fr_1.2fr]">
                  <GlassCard>
                    <div className="mb-4 flex h-14 w-14 items-center justify-center rounded-full bg-pink-500/15">
                      <Lock className="h-7 w-7 text-pink-600" />
                    </div>
                    <h2 className="text-3xl font-black text-rose-700">Open when you miss me</h2>
                    <p className="mt-5 text-lg leading-8 text-rose-800">
                      Whenever you miss me, open this again and remember that somewhere far away, there is one person thinking about you, praying for you, and waiting for the day he can celebrate you in person.
                    </p>
                  </GlassCard>

                  <GlassCard>
                    <div className="mb-4 flex h-14 w-14 items-center justify-center rounded-full bg-fuchsia-500/15">
                      <Gift className="h-7 w-7 text-fuchsia-600" />
                    </div>
                    <h2 className="text-3xl font-black text-rose-700">Small promises for your new year</h2>
                    <div className="mt-5 space-y-4">
                      {promises.map((promise) => (
                        <div key={promise} className="rounded-3xl bg-white/35 p-4 text-rose-800 shadow-sm">
                          {promise}
                        </div>
                      ))}
                    </div>
                  </GlassCard>
                </div>
              </Section>

              <Section>
                <GlassCard className="text-center">
                  <motion.div animate={{ scale: [1, 1.08, 1] }} transition={{ repeat: Infinity, duration: 2 }} className="mx-auto mb-6 flex h-20 w-20 items-center justify-center rounded-full bg-gradient-to-r from-pink-500 to-fuchsia-500 text-white shadow-2xl shadow-pink-500/30">
                    <Music className="h-10 w-10" />
                  </motion.div>
                  <h2 className="text-4xl font-black text-rose-700 sm:text-5xl">Now call me, birthday girl</h2>
                  <p className="mx-auto mt-6 max-w-2xl text-lg leading-9 text-rose-800">
                    I made this for you, but the real surprise is waiting on video call. Come see your cake. Make a wish. Let me be part of the first few minutes of your birthday.
                  </p>
                  <div className="mt-9 rounded-[2rem] bg-gradient-to-r from-pink-500 via-fuchsia-500 to-rose-500 p-1 shadow-2xl shadow-pink-500/25">
                    <div className="rounded-[1.8rem] bg-white/80 px-6 py-8 text-2xl font-black text-pink-600 backdrop-blur-xl">
                      Happy Birthday, Misty. I love you. ❤
                    </div>
                  </div>
                </GlassCard>
              </Section>
            </motion.div>
          )}
        </AnimatePresence>
      </div>
    </main>
  );
}
