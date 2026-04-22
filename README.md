import { useState, useEffect, useRef } from “react”;

const BRAND = {
name: “Le Grand Bain”,
tagline: “Du bord au large — en 6 semaines.”,
sub: “Le premier programme audio conçu par un MNS pour passer de l’anxiété aquatique à la liberté totale dans l’eau.”,
};

const WEEKS = [
{
num: 1,
title: “Retrouver la sensation”,
color: “#0EA5E9”,
gradient: “from-sky-400 to-cyan-500”,
icon: “🌊”,
objectif: “Se sentir à l’aise en eau peu profonde sans panique”,
seances: [
{
id: “s1-1”,
titre: “Prise de contact”,
duree: “20 min”,
profil: [“Anxieux”, “Non-nageur”, “Rouillé”],
desc: “Marche lente dans l’eau, arrêts conscients, respirations profondes. Petit bain uniquement. Aucune immersion imposée.”,
exercices: [“Marche 10 allers-retours petit bain”, “Arrêts de 10 secondes, yeux fermés”, “3 cycles de respiration abdominale”],
audio: “Voix calme, rythme lent. Guidage de la respiration en marchant. Durée : 8 min.”,
consigne: “Note ton score de confiance de 1 à 10. Écris un mot qui décrit ton ressenti.”,
},
{
id: “s1-2”,
titre: “Jeu avec l’eau”,
duree: “20 min”,
profil: [“Anxieux”, “Rouillé”],
desc: “Éclaboussures volontaires, visage mouillé progressivement. On apprivoise — pas de performance.”,
exercices: [“Éclabousser ses bras, puis son visage”, “Souffler dans l’eau 5 fois de suite”, “Rester immobile 30 secondes”],
audio: “Ton ludique et encourageant. Exercice guidé : souffler des bulles. Durée : 6 min.”,
consigne: “Ton score a-t-il changé vs séance 1 ? Qu’est-ce qui a aidé ?”,
},
{
id: “s1-3”,
titre: “Flottaison assistée”,
duree: “25 min”,
profil: [“Anxieux”, “Non-nageur”, “Rouillé”],
desc: “Position dorsale avec appui de la nuque. Relâchement progressif. Focus sur la respiration abdominale.”,
exercices: [“Allongement dorsal avec appui bord”, “Relâcher les épaules, puis les hanches”, “Tenir 10 secondes sans tension”],
audio: “Guidage scan corporel : pieds → mollets → hanches → épaules. Durée : 10 min.”,
consigne: “Combien de secondes as-tu flotté sans tension ? Objectif : 10 sec.”,
},
],
},
{
num: 2,
title: “Maîtriser sa respiration”,
color: “#06B6D4”,
gradient: “from-cyan-500 to-teal-500”,
icon: “💨”,
objectif: “Contrôler l’apnée courte et l’expiration dans l’eau”,
seances: [
{
id: “s2-1”,
titre: “Expiration subaquatique”,
duree: “20 min”,
profil: [“Anxieux”, “Non-nageur”, “Rouillé”],
desc: “Souffler sous l’eau, bulles par le nez et la bouche. 3 séries de 10 expirations.”,
exercices: [“10 expirations nez dans l’eau”, “10 expirations bouche dans l’eau”, “10 expirations tête entièrement immergée”],
audio: “Comptage lent : inspir 3 temps, expir 5 temps sous l’eau. Durée : 8 min.”,
consigne: “As-tu senti de l’anxiété en immergeant le visage ? Note l’intensité (1-10).”,
},
{
id: “s2-2”,
titre: “Apnée statique au bord”,
duree: “20 min”,
profil: [“Anxieux”, “Rouillé”],
desc: “Tenu du bord, immersion du visage, apnée 5 secondes. Progresser vers 10 sec.”,
exercices: [“5 apnées de 5 secondes”, “3 apnées de 8 secondes”, “2 apnées de 10 secondes”],
audio: “Compte à rebours calme : 5, 4, 3, 2, 1 — remonte, inspire. Durée : 7 min.”,
consigne: “Temps maximum d’apnée atteint aujourd’hui ?”,
},
{
id: “s2-3”,
titre: “Cycle complet en déplacement”,
duree: “25 min”,
profil: [“Anxieux”, “Rouillé”, “Non-nageur”],
desc: “Crawl simplifié : 3 bras, 1 respiration. Distance : 10m max.”,
exercices: [“3 x 10m crawl débutant”, “Focus : tourner la tête, pas lever la tête”, “Repos 30 sec entre chaque longueur”],
audio: “Guidage rythmique : bras, bras, bras, tourne, expire, inspire. Durée : 9 min.”,
consigne: “Sur 10m, combien de cycles respiratoires sans panique ?”,
},
],
},
{
num: 3,
title: “Conquérir la profondeur”,
color: “#0284C7”,
gradient: “from-sky-600 to-blue-700”,
icon: “⬇️”,
objectif: “Se sentir en sécurité en eau profonde sans appui”,
seances: [
{
id: “s3-1”,
titre: “Franchir la ligne”,
duree: “25 min”,
profil: [“Anxieux”, “Rouillé”],
desc: “Traverser la ligne de profondeur en tenant le couloir. S’arrêter, observer, repartir.”,
exercices: [“5 franchissements ligne petit/grand bain”, “Arrêt de 15 sec en eau profonde, bord tenu”, “3 franchissements sans s’arrêter”],
audio: “Ancrage : ‘Tu flottes. L’eau te porte. Tu contrôles ta respiration.’ Durée : 8 min.”,
consigne: “As-tu franchi la ligne ? Combien de fois ? Qu’est-ce qui a aidé ?”,
},
{
id: “s3-2”,
titre: “Flottaison en eau profonde”,
duree: “25 min”,
profil: [“Anxieux”, “Rouillé”],
desc: “Lâcher le bord, flotter 15 secondes en étoile. Seul, sans appui.”,
exercices: [“3 x flottaison étoile 10 sec”, “3 x flottaison étoile 15 sec”, “1 x flottaison dorsale 20 sec”],
audio: “Respiration guidée longue. Rappel : ‘Tu n’as pas besoin du fond.’ Durée : 10 min.”,
consigne: “Score de confiance avant/après. Différence observée ?”,
},
{
id: “s3-3”,
titre: “Nage en eau profonde”,
duree: “30 min”,
profil: [“Anxieux”, “Rouillé”, “Non-nageur”],
desc: “2 x 25m en crawl ou brasse, sans s’arrêter, sans toucher le bord entre les deux.”,
exercices: [“1 x 25m crawl ou brasse”, “Repos 45 sec”, “1 x 25m retour sans toucher le bord”],
audio: “Encouragement à mi-parcours : ‘Continue, tu gères, rythme régulier.’ Durée : 8 min.”,
consigne: “As-tu ressenti l’envie de t’arrêter ? Qu’est-ce qui t’a aidé à continuer ?”,
},
],
},
{
num: 4,
title: “Construire l’endurance”,
color: “#0369A1”,
gradient: “from-blue-700 to-indigo-700”,
icon: “💪”,
objectif: “Maintenir la sérénité sur des efforts plus longs”,
seances: [
{
id: “s4-1”,
titre: “Nage continue 200m”,
duree: “30 min”,
profil: [“Anxieux”, “Rouillé”],
desc: “Nager 200m sans s’arrêter, rythme lent, technique secondaire. Seule la continuité compte.”,
exercices: [“4 x 50m avec 20 sec repos”, “ou 200m non-stop si possible”, “Focus : régularité du souffle”],
audio: “Mindfulness aquatique : sens l’eau sur ta peau, tes bras, ton souffle. Durée : 12 min.”,
consigne: “As-tu terminé les 200m ? Quelle était ta principale difficulté ?”,
},
{
id: “s4-2”,
titre: “Jeu de profondeur”,
duree: “25 min”,
profil: [“Anxieux”, “Rouillé”, “Non-nageur”],
desc: “Plonger pour récupérer un objet au fond (1,5m). 5 tentatives. Pas d’obligation de réussir.”,
exercices: [“Lâcher un objet en eau peu profonde”, “Récupérer l’objet en eau profonde”, “5 tentatives, sans pression de résultat”],
audio: “Préparation mentale avant chaque plongeon : 3 respirations profondes. Durée : 9 min.”,
consigne: “Combien de tentatives réussies ? Qu’est-ce qui a changé entre la 1ère et la 5ème ?”,
},
{
id: “s4-3”,
titre: “Nage en open space”,
duree: “30 min”,
profil: [“Anxieux”, “Rouillé”],
desc: “Nager dans le grand bain sans suivre le couloir. Trajectoire libre. Objectif : se repérer sans anxiété.”,
exercices: [“Nage diagonale grand bain x3”, “Nage en cercle autour du centre”, “Arrêt statique au milieu du bassin”],
audio: “Guidage orientation : ‘Regarde devant, pas en bas. Tu maîtrises l’espace.’ Durée : 10 min.”,
consigne: “Comment tu te sens par rapport à la semaine 1 ? Écris 3 mots.”,
},
],
},
{
num: 5,
title: “Situations inattendues”,
color: “#1D4ED8”,
gradient: “from-indigo-700 to-violet-700”,
icon: “⚡”,
objectif: “Garder le calme face à l’inattendu dans l’eau”,
seances: [
{
id: “s5-1”,
titre: “Chute simulée”,
duree: “25 min”,
profil: [“Anxieux”, “Rouillé”],
desc: “Entrer dans l’eau sans s’accrocher, comme une chute. Retrouver l’équilibre seul. 5 fois.”,
exercices: [“5 entrées eau sans appui”, “Retrouver surface immédiatement”, “Nager 10m après chaque entrée”],
audio: “Post-chute : ‘Respire, oriente-toi, nage vers le bord. Tu sais faire.’ Durée : 8 min.”,
consigne: “As-tu eu une montée de stress ? À quel moment exactement ?”,
},
{
id: “s5-2”,
titre: “Nage en aveugle partiel”,
duree: “25 min”,
profil: [“Rouillé”, “Anxieux”],
desc: “Nager 25m sans lunettes. S’adapter à la désorientation visuelle.”,
exercices: [“25m sans lunettes, yeux ouverts”, “25m sans lunettes, yeux mi-fermés”, “Focus : sensations corporelles uniquement”],
audio: “Guidage proprioceptif : ‘Sens la résistance de l’eau, pas besoin de voir le fond.’ Durée : 7 min.”,
consigne: “Quelle est la principale sensation nouvelle aujourd’hui ?”,
},
{
id: “s5-3”,
titre: “Consolidation autonome”,
duree: “30 min”,
profil: [“Anxieux”, “Rouillé”, “Non-nageur”],
desc: “Reproduire les exercices clés des semaines 3 et 4 de mémoire, sans consigne audio.”,
exercices: [“Flottaison étoile 20 sec”, “2 x 50m continus”, “1 plongeon récupération objet”],
audio: “Pas d’audio cette séance. Test d’autonomie complète.”,
consigne: “Note globale de confiance à ce stade (1-10). Tu es où par rapport à ton objectif de départ ?”,
},
],
},
{
num: 6,
title: “Autonomie totale”,
color: “#4F46E5”,
gradient: “from-violet-700 to-purple-800”,
icon: “🏆”,
objectif: “Nager seul, en confiance, dans n’importe quelle situation”,
seances: [
{
id: “s6-1”,
titre: “Séance libre guidée”,
duree: “30 min”,
profil: [“Anxieux”, “Rouillé”, “Non-nageur”],
desc: “Tu choisis tes exercices parmi ceux du programme. 30 min. Aucune contrainte imposée.”,
exercices: [“Choix libre parmi les exercices précédents”, “Objectif auto-défini”, “Écoute de ton corps en priorité”],
audio: “Intro motivationnelle 2 min. Le reste : silence choisi.”,
consigne: “Qu’est-ce que tu as choisi de faire, et pourquoi ?”,
},
{
id: “s6-2”,
titre: “Test 400m”,
duree: “35 min”,
profil: [“Anxieux”, “Rouillé”],
desc: “400m nage continue. Chronométrer. L’objectif n’est pas la vitesse — c’est terminer sereinement.”,
exercices: [“400m nage continue (toute nage)”, “Chronométrer sans pression”, “Focus : sérénité sur toute la distance”],
audio: “Débrief final post-séance : félicitations, ancrage de progression. Durée : 5 min.”,
consigne: “Compare ton score de confiance semaine 1 vs semaine 6. Qu’est-ce qui a changé ?”,
},
{
id: “s6-3”,
titre: “Séance d’ouverture”,
duree: “Variable”,
profil: [“Anxieux”, “Rouillé”, “Non-nageur”],
desc: “Essayer un nouvel environnement : lac, mer, piscine inconnue. Transférer les acquis.”,
exercices: [“Nager dans un lieu nouveau”, “Appliquer les techniques du programme”, “Identifier ce qui a changé en toi”],
audio: “Préparation mentale : ‘Tu as les outils. Ce lieu est nouveau, tes sensations ne le sont plus.’ Durée : 6 min.”,
consigne: “Bilan final : 3 victoires du programme. Ce que tu veux continuer à travailler.”,
},
],
},
];

const PROFIL_COLORS = {
“Anxieux”: “bg-rose-100 text-rose-700 border border-rose-200”,
“Non-nageur”: “bg-amber-100 text-amber-700 border border-amber-200”,
“Rouillé”: “bg-teal-100 text-teal-700 border border-teal-200”,
};

const CheckIcon = () => (
<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2.5" strokeLinecap="round" strokeLinejoin="round" className="w-4 h-4">
<polyline points="20 6 9 17 4 12" />
</svg>
);

const WaveIcon = () => (
<svg viewBox="0 0 80 20" className="w-20 h-5 opacity-30">
<path d="M0 10 Q10 0 20 10 Q30 20 40 10 Q50 0 60 10 Q70 20 80 10" stroke="currentColor" strokeWidth="2" fill="none"/>
</svg>
);

export default function LeGrandBain() {
const [view, setView] = useState(“home”);
const [selectedWeek, setSelectedWeek] = useState(null);
const [selectedSeance, setSelectedSeance] = useState(null);
const [completed, setCompleted] = useState(() => {
try { return JSON.parse(localStorage.getItem(“lgb_completed”) || “{}”); } catch { return {}; }
});
const [scores, setScores] = useState(() => {
try { return JSON.parse(localStorage.getItem(“lgb_scores”) || “{}”); } catch { return {}; }
});
const [notes, setNotes] = useState(() => {
try { return JSON.parse(localStorage.getItem(“lgb_notes”) || “{}”); } catch { return {}; }
});
const [tempScore, setTempScore] = useState(5);
const [tempNote, setTempNote] = useState(””);
const [checkinDone, setCheckinDone] = useState(false);
const [animated, setAnimated] = useState(false);

useEffect(() => {
setTimeout(() => setAnimated(true), 100);
}, []);

const totalSeances = WEEKS.reduce((a, w) => a + w.seances.length, 0);
const completedCount = Object.keys(completed).filter(k => completed[k]).length;
const progress = Math.round((completedCount / totalSeances) * 100);

const currentWeekIndex = WEEKS.findIndex(w => w.seances.some(s => !completed[s.id]));
const currentWeek = currentWeekIndex >= 0 ? WEEKS[currentWeekIndex] : WEEKS[5];

const saveCheckin = (seanceId) => {
const newScores = { …scores, [seanceId]: tempScore };
const newNotes = { …notes, [seanceId]: tempNote };
const newCompleted = { …completed, [seanceId]: true };
setScores(newScores);
setNotes(newNotes);
setCompleted(newCompleted);
try {
localStorage.setItem(“lgb_scores”, JSON.stringify(newScores));
localStorage.setItem(“lgb_notes”, JSON.stringify(newNotes));
localStorage.setItem(“lgb_completed”, JSON.stringify(newCompleted));
} catch {}
setCheckinDone(true);
};

const avgScore = () => {
const vals = Object.values(scores);
if (!vals.length) return null;
return (vals.reduce((a, b) => a + b, 0) / vals.length).toFixed(1);
};

const openSeance = (week, seance) => {
setSelectedWeek(week);
setSelectedSeance(seance);
setTempScore(scores[seance.id] || 5);
setTempNote(notes[seance.id] || “”);
setCheckinDone(!!completed[seance.id]);
setView(“seance”);
};

// ─── HOME ───────────────────────────────────────────────────────
if (view === “home”) return (
<div className=“min-h-screen bg-slate-950 text-white font-sans” style={{ fontFamily: “‘DM Sans’, sans-serif” }}>
<style>{`@import url('https://fonts.googleapis.com/css2?family=DM+Sans:ital,wght@0,300;0,400;0,500;0,700;1,300&family=Playfair+Display:ital,wght@0,700;1,600&display=swap'); * { box-sizing: border-box; } .fade-up { opacity: 0; transform: translateY(24px); transition: all 0.7s cubic-bezier(.22,1,.36,1); } .fade-up.show { opacity: 1; transform: translateY(0); } .wave-bg { background: radial-gradient(ellipse 80% 60% at 50% -10%, rgba(14,165,233,0.18) 0%, transparent 70%); } .glass { background: rgba(255,255,255,0.04); border: 1px solid rgba(255,255,255,0.08); backdrop-filter: blur(12px); } .week-card { transition: all 0.3s ease; cursor: pointer; } .week-card:hover { transform: translateY(-4px); } .score-btn { transition: all 0.2s; border: 2px solid rgba(255,255,255,0.15); } .score-btn:hover { border-color: #0EA5E9; transform: scale(1.1); } .score-btn.active { background: #0EA5E9; border-color: #0EA5E9; } .progress-bar { transition: width 1.2s cubic-bezier(.22,1,.36,1); } ::-webkit-scrollbar { width: 4px; } ::-webkit-scrollbar-track { background: transparent; } ::-webkit-scrollbar-thumb { background: rgba(14,165,233,0.3); border-radius: 2px; } .nav-btn { transition: all 0.2s; } .nav-btn:hover { background: rgba(14,165,233,0.15); } .nav-btn.active { background: rgba(14,165,233,0.2); color: #38BDF8; } .checkin-complete { animation: pop 0.4s cubic-bezier(.34,1.56,.64,1); } @keyframes pop { 0% { transform: scale(0.8); opacity: 0; } 100% { transform: scale(1); opacity: 1; } }`}</style>

```
  {/* HERO */}
  <div className="wave-bg relative overflow-hidden">
    <div className="max-w-2xl mx-auto px-6 pt-16 pb-12">
      <div className={`fade-up ${animated ? "show" : ""}`} style={{ transitionDelay: "0ms" }}>
        <div className="flex items-center gap-2 mb-6">
          <div className="w-8 h-8 rounded-full bg-sky-500 flex items-center justify-center text-sm">🌊</div>
          <span className="text-sky-400 text-sm font-medium tracking-widest uppercase">Le Grand Bain</span>
        </div>
        <h1 className="text-5xl font-bold leading-tight mb-3" style={{ fontFamily: "'Playfair Display', serif" }}>
          Du bord<br /><span className="text-sky-400 italic">au large.</span>
        </h1>
        <p className="text-slate-400 text-lg leading-relaxed mb-8 max-w-md">
          {BRAND.sub}
        </p>
      </div>

      {/* PROGRESS CARD */}
      <div className={`glass rounded-2xl p-6 mb-6 fade-up ${animated ? "show" : ""}`} style={{ transitionDelay: "150ms" }}>
        <div className="flex justify-between items-center mb-3">
          <span className="text-slate-300 text-sm font-medium">Ta progression</span>
          <span className="text-sky-400 font-bold text-lg">{completedCount}<span className="text-slate-500 text-sm font-normal">/{totalSeances} séances</span></span>
        </div>
        <div className="h-2 bg-slate-800 rounded-full overflow-hidden mb-4">
          <div className="progress-bar h-full bg-gradient-to-r from-sky-500 to-cyan-400 rounded-full" style={{ width: `${progress}%` }} />
        </div>
        <div className="flex gap-4">
          <div className="flex-1 text-center">
            <div className="text-2xl font-bold text-white">{progress}%</div>
            <div className="text-slate-500 text-xs mt-0.5">Complété</div>
          </div>
          <div className="w-px bg-slate-800" />
          <div className="flex-1 text-center">
            <div className="text-2xl font-bold text-white">{avgScore() || "—"}</div>
            <div className="text-slate-500 text-xs mt-0.5">Score moyen</div>
          </div>
          <div className="w-px bg-slate-800" />
          <div className="flex-1 text-center">
            <div className="text-2xl font-bold text-white">{currentWeek.num}</div>
            <div className="text-slate-500 text-xs mt-0.5">Semaine active</div>
          </div>
        </div>
      </div>

      {/* CTA */}
      <div className={`fade-up ${animated ? "show" : ""}`} style={{ transitionDelay: "250ms" }}>
        <button
          onClick={() => setView("programme")}
          className="w-full bg-sky-500 hover:bg-sky-400 text-white font-semibold py-4 rounded-2xl transition-all duration-200 text-base mb-3"
        >
          Continuer le programme →
        </button>
        <button
          onClick={() => setView("profil")}
          className="w-full glass text-slate-300 hover:text-white font-medium py-3 rounded-2xl transition-all duration-200 text-sm"
        >
          Choisir mon profil
        </button>
      </div>
    </div>
  </div>

  {/* CURRENT WEEK HIGHLIGHT */}
  <div className="max-w-2xl mx-auto px-6 py-8">
    <div className={`fade-up ${animated ? "show" : ""}`} style={{ transitionDelay: "350ms" }}>
      <p className="text-slate-500 text-xs uppercase tracking-widest mb-4 font-medium">Semaine en cours</p>
      <div
        className="glass rounded-2xl p-5 week-card"
        onClick={() => setView("programme")}
      >
        <div className="flex items-center gap-4">
          <div className="w-14 h-14 rounded-2xl flex items-center justify-center text-2xl" style={{ background: `${currentWeek.color}22` }}>
            {currentWeek.icon}
          </div>
          <div className="flex-1">
            <div className="text-xs text-slate-500 mb-0.5">Semaine {currentWeek.num}</div>
            <div className="font-semibold text-white text-base">{currentWeek.title}</div>
            <div className="text-slate-400 text-sm mt-0.5">{currentWeek.objectif}</div>
          </div>
          <div className="text-slate-600">→</div>
        </div>
        <div className="mt-4 flex gap-2">
          {currentWeek.seances.map(s => (
            <div key={s.id} className={`flex-1 h-1.5 rounded-full ${completed[s.id] ? "bg-sky-500" : "bg-slate-800"}`} />
          ))}
        </div>
      </div>
    </div>
  </div>

  {/* NAV */}
  <div className="fixed bottom-0 left-0 right-0 bg-slate-950 border-t border-slate-800 px-6 py-3">
    <div className="max-w-2xl mx-auto flex gap-2">
      {[
        { id: "home", label: "Accueil", icon: "🏠" },
        { id: "programme", label: "Programme", icon: "📅" },
        { id: "journal", label: "Journal", icon: "📊" },
        { id: "profil", label: "Profil", icon: "👤" },
      ].map(tab => (
        <button key={tab.id} onClick={() => setView(tab.id)}
          className={`nav-btn flex-1 py-2 rounded-xl text-center ${view === tab.id ? "active" : ""}`}>
          <div className="text-lg">{tab.icon}</div>
          <div className="text-xs text-slate-500">{tab.label}</div>
        </button>
      ))}
    </div>
  </div>
  <div className="h-20" />
</div>
```

);

// ─── PROGRAMME ──────────────────────────────────────────────────
if (view === “programme”) return (
<div className=“min-h-screen bg-slate-950 text-white” style={{ fontFamily: “‘DM Sans’, sans-serif” }}>
<style>{`@import url('https://fonts.googleapis.com/css2?family=DM+Sans:ital,wght@0,300;0,400;0,500;0,700;1,300&family=Playfair+Display:ital,wght@0,700;1,600&display=swap'); * { box-sizing: border-box; } .glass { background: rgba(255,255,255,0.04); border: 1px solid rgba(255,255,255,0.08); } .week-card { transition: all 0.3s ease; cursor: pointer; } .week-card:hover { transform: translateY(-3px); } .nav-btn { transition: all 0.2s; } .nav-btn:hover { background: rgba(14,165,233,0.15); } .nav-btn.active { background: rgba(14,165,233,0.2); color: #38BDF8; } ::-webkit-scrollbar { width: 4px; } ::-webkit-scrollbar-thumb { background: rgba(14,165,233,0.3); border-radius: 2px; }`}</style>

```
  <div className="max-w-2xl mx-auto px-6 pt-10 pb-24">
    <div className="flex items-center justify-between mb-8">
      <div>
        <p className="text-slate-500 text-xs uppercase tracking-widest mb-1">Le Grand Bain</p>
        <h2 className="text-2xl font-bold" style={{ fontFamily: "'Playfair Display', serif" }}>Programme</h2>
      </div>
      <div className="glass rounded-xl px-4 py-2 text-center">
        <div className="text-sky-400 font-bold">{completedCount}/{totalSeances}</div>
        <div className="text-slate-500 text-xs">séances</div>
      </div>
    </div>

    <div className="space-y-4">
      {WEEKS.map((week, wi) => {
        const weekCompleted = week.seances.filter(s => completed[s.id]).length;
        const isActive = week.num === currentWeek.num;
        return (
          <div key={week.num} className="glass rounded-2xl overflow-hidden week-card">
            <div className="p-5">
              <div className="flex items-center gap-4 mb-4">
                <div className="w-12 h-12 rounded-xl flex items-center justify-center text-xl" style={{ background: `${week.color}22` }}>
                  {weekCompleted === 3 ? "✅" : week.icon}
                </div>
                <div className="flex-1">
                  <div className="flex items-center gap-2">
                    <span className="text-xs text-slate-500">Semaine {week.num}</span>
                    {isActive && <span className="text-xs bg-sky-500/20 text-sky-400 px-2 py-0.5 rounded-full border border-sky-500/30">En cours</span>}
                    {weekCompleted === 3 && <span className="text-xs bg-emerald-500/20 text-emerald-400 px-2 py-0.5 rounded-full border border-emerald-500/30">Terminée</span>}
                  </div>
                  <div className="font-semibold text-white">{week.title}</div>
                </div>
                <div className="text-slate-600 text-sm">{weekCompleted}/3</div>
              </div>

              <div className="h-1 bg-slate-800 rounded-full overflow-hidden mb-4">
                <div className="h-full rounded-full transition-all duration-700" style={{ width: `${(weekCompleted / 3) * 100}%`, background: week.color }} />
              </div>

              <div className="space-y-2">
                {week.seances.map((seance, si) => (
                  <button key={seance.id} onClick={() => openSeance(week, seance)}
                    className="w-full flex items-center gap-3 p-3 rounded-xl hover:bg-white/5 transition-all duration-200 text-left">
                    <div className={`w-7 h-7 rounded-full flex items-center justify-center text-xs flex-shrink-0 ${completed[seance.id] ? "bg-sky-500 text-white" : "bg-slate-800 text-slate-500"}`}>
                      {completed[seance.id] ? <CheckIcon /> : si + 1}
                    </div>
                    <div className="flex-1">
                      <div className="text-sm font-medium text-white">{seance.titre}</div>
                      <div className="text-xs text-slate-500">{seance.duree}</div>
                    </div>
                    {scores[seance.id] && (
                      <div className="text-xs text-sky-400 font-medium">{scores[seance.id]}/10</div>
                    )}
                    <div className="text-slate-700 text-xs">→</div>
                  </button>
                ))}
              </div>
            </div>
          </div>
        );
      })}
    </div>
  </div>

  <div className="fixed bottom-0 left-0 right-0 bg-slate-950 border-t border-slate-800 px-6 py-3">
    <div className="max-w-2xl mx-auto flex gap-2">
      {[{ id: "home", label: "Accueil", icon: "🏠" }, { id: "programme", label: "Programme", icon: "📅" }, { id: "journal", label: "Journal", icon: "📊" }, { id: "profil", label: "Profil", icon: "👤" }].map(tab => (
        <button key={tab.id} onClick={() => setView(tab.id)} className={`nav-btn flex-1 py-2 rounded-xl text-center ${view === tab.id ? "active" : ""}`}>
          <div className="text-lg">{tab.icon}</div>
          <div className="text-xs text-slate-500">{tab.label}</div>
        </button>
      ))}
    </div>
  </div>
</div>
```

);

// ─── SÉANCE ─────────────────────────────────────────────────────
if (view === “seance” && selectedSeance) return (
<div className=“min-h-screen bg-slate-950 text-white” style={{ fontFamily: “‘DM Sans’, sans-serif” }}>
<style>{`@import url('https://fonts.googleapis.com/css2?family=DM+Sans:ital,wght@0,300;0,400;0,500;0,700;1,300&family=Playfair+Display:ital,wght@0,700;1,600&display=swap'); * { box-sizing: border-box; } .glass { background: rgba(255,255,255,0.04); border: 1px solid rgba(255,255,255,0.08); } .score-btn { transition: all 0.2s; border: 2px solid rgba(255,255,255,0.1); border-radius: 12px; } .score-btn:hover { border-color: #0EA5E9; transform: scale(1.08); } .score-btn.sel { background: #0EA5E9; border-color: #0EA5E9; } .checkin-complete { animation: pop 0.4s cubic-bezier(.34,1.56,.64,1); } @keyframes pop { 0% { transform: scale(0.8); opacity:0; } 100% { transform: scale(1); opacity:1; } }`}</style>

```
  {/* Header */}
  <div className="relative overflow-hidden" style={{ background: `linear-gradient(135deg, ${selectedWeek.color}33 0%, transparent 60%)` }}>
    <div className="max-w-2xl mx-auto px-6 pt-8 pb-6">
      <button onClick={() => setView("programme")} className="flex items-center gap-2 text-slate-400 hover:text-white text-sm mb-6 transition-colors">
        ← Retour
      </button>
      <div className="flex items-center gap-3 mb-4">
        <div className="w-10 h-10 rounded-xl flex items-center justify-center text-lg" style={{ background: `${selectedWeek.color}33` }}>
          {selectedWeek.icon}
        </div>
        <div>
          <div className="text-xs text-slate-500">Semaine {selectedWeek.num} · {selectedWeek.title}</div>
          <div className="text-slate-400 text-sm">{selectedSeance.duree}</div>
        </div>
      </div>
      <h1 className="text-3xl font-bold mb-2" style={{ fontFamily: "'Playfair Display', serif" }}>{selectedSeance.titre}</h1>
      <p className="text-slate-400 leading-relaxed">{selectedSeance.desc}</p>
      <div className="flex flex-wrap gap-2 mt-3">
        {selectedSeance.profil.map(p => (
          <span key={p} className={`text-xs px-2 py-1 rounded-full ${PROFIL_COLORS[p]}`}>{p}</span>
        ))}
      </div>
    </div>
  </div>

  <div className="max-w-2xl mx-auto px-6 pb-24 space-y-5">
    {/* Exercices */}
    <div className="glass rounded-2xl p-5">
      <h3 className="font-semibold text-white mb-4 flex items-center gap-2">
        <span className="w-6 h-6 rounded-full bg-sky-500/20 text-sky-400 text-xs flex items-center justify-center">💧</span>
        Exercices
      </h3>
      <div className="space-y-3">
        {selectedSeance.exercices.map((ex, i) => (
          <div key={i} className="flex items-start gap-3">
            <div className="w-5 h-5 rounded-full bg-slate-800 text-slate-500 text-xs flex items-center justify-center flex-shrink-0 mt-0.5">{i + 1}</div>
            <p className="text-slate-300 text-sm leading-relaxed">{ex}</p>
          </div>
        ))}
      </div>
    </div>

    {/* Audio */}
    <div className="glass rounded-2xl p-5">
      <h3 className="font-semibold text-white mb-3 flex items-center gap-2">
        <span className="w-6 h-6 rounded-full bg-violet-500/20 text-violet-400 text-xs flex items-center justify-center">🎙</span>
        Audio guidé
      </h3>
      <div className="bg-slate-900 rounded-xl p-4 flex items-center gap-4">
        <div className="w-10 h-10 rounded-full bg-sky-500 flex items-center justify-center text-white flex-shrink-0">▶</div>
        <div>
          <div className="text-sm font-medium text-white">Écouter avant d'entrer dans l'eau</div>
          <div className="text-xs text-slate-500 mt-0.5">{selectedSeance.audio}</div>
        </div>
      </div>
    </div>

    {/* Check-in */}
    <div className="glass rounded-2xl p-5">
      <h3 className="font-semibold text-white mb-1 flex items-center gap-2">
        <span className="w-6 h-6 rounded-full bg-emerald-500/20 text-emerald-400 text-xs flex items-center justify-center">✓</span>
        Check-in post-séance
      </h3>
      <p className="text-slate-500 text-xs mb-4">À remplir juste après ta séance</p>

      {checkinDone ? (
        <div className="checkin-complete text-center py-6">
          <div className="text-4xl mb-2">✅</div>
          <div className="text-white font-semibold">Séance validée !</div>
          <div className="text-slate-400 text-sm mt-1">Score : {scores[selectedSeance.id]}/10</div>
          {notes[selectedSeance.id] && <div className="text-slate-500 text-xs mt-2 italic">"{notes[selectedSeance.id]}"</div>}
          <button onClick={() => setCheckinDone(false)} className="mt-4 text-xs text-slate-600 underline">Modifier</button>
        </div>
      ) : (
        <div>
          <p className="text-sm text-slate-400 mb-3">Score de confiance</p>
          <div className="grid grid-cols-5 gap-2 mb-5">
            {[1,2,3,4,5,6,7,8,9,10].map(n => (
              <button key={n} onClick={() => setTempScore(n)}
                className={`score-btn py-2 text-sm font-semibold ${tempScore === n ? "sel text-white" : "text-slate-400"}`}>
                {n}
              </button>
            ))}
          </div>
          <p className="text-sm text-slate-400 mb-2">{selectedSeance.consigne}</p>
          <textarea
            value={tempNote}
            onChange={e => setTempNote(e.target.value)}
            placeholder="Ton observation..."
            rows={3}
            className="w-full bg-slate-900 border border-slate-800 rounded-xl p-3 text-sm text-white placeholder-slate-600 focus:outline-none focus:border-sky-500 resize-none"
          />
          <button onClick={() => saveCheckin(selectedSeance.id)}
            className="w-full mt-3 bg-sky-500 hover:bg-sky-400 text-white font-semibold py-3 rounded-xl transition-all duration-200">
            Valider la séance
          </button>
        </div>
      )}
    </div>
  </div>
</div>
```

);

// ─── JOURNAL ────────────────────────────────────────────────────
if (view === “journal”) return (
<div className=“min-h-screen bg-slate-950 text-white” style={{ fontFamily: “‘DM Sans’, sans-serif” }}>
<style>{`@import url('https://fonts.googleapis.com/css2?family=DM+Sans:ital,wght@0,300;0,400;0,500;0,700;1,300&family=Playfair+Display:ital,wght@0,700;1,600&display=swap'); * { box-sizing: border-box; } .glass { background: rgba(255,255,255,0.04); border: 1px solid rgba(255,255,255,0.08); } .nav-btn { transition: all 0.2s; } .nav-btn.active { background: rgba(14,165,233,0.2); color: #38BDF8; }`}</style>

```
  <div className="max-w-2xl mx-auto px-6 pt-10 pb-24">
    <p className="text-slate-500 text-xs uppercase tracking-widest mb-1">Le Grand Bain</p>
    <h2 className="text-2xl font-bold mb-8" style={{ fontFamily: "'Playfair Display', serif" }}>Journal de confiance</h2>

    {/* Stats */}
    <div className="grid grid-cols-3 gap-3 mb-6">
      {[
        { label: "Séances", value: completedCount, unit: `/${totalSeances}`, color: "text-sky-400" },
        { label: "Score moyen", value: avgScore() || "—", unit: "/10", color: "text-emerald-400" },
        { label: "Semaine", value: currentWeek.num, unit: "/6", color: "text-violet-400" },
      ].map(stat => (
        <div key={stat.label} className="glass rounded-2xl p-4 text-center">
          <div className={`text-2xl font-bold ${stat.color}`}>{stat.value}<span className="text-slate-600 text-sm">{stat.unit}</span></div>
          <div className="text-slate-500 text-xs mt-1">{stat.label}</div>
        </div>
      ))}
    </div>

    {/* Score Timeline */}
    {Object.keys(scores).length > 0 && (
      <div className="glass rounded-2xl p-5 mb-5">
        <h3 className="font-semibold text-white mb-4">Évolution du score</h3>
        <div className="flex items-end gap-2 h-24">
          {WEEKS.flatMap(w => w.seances).map(s => {
            const score = scores[s.id];
            return (
              <div key={s.id} className="flex-1 flex flex-col items-center gap-1">
                <div className="w-full rounded-t-sm transition-all duration-700"
                  style={{ height: score ? `${(score / 10) * 80}px` : "4px", background: score ? "#0EA5E9" : "#1e293b" }} />
                {score && <div className="text-xs text-slate-600">{score}</div>}
              </div>
            );
          })}
        </div>
        <div className="flex justify-between mt-1">
          <span className="text-xs text-slate-600">S1</span>
          <span className="text-xs text-slate-600">S6</span>
        </div>
      </div>
    )}

    {/* Notes */}
    <div className="space-y-3">
      <h3 className="font-semibold text-white text-sm">Tes observations</h3>
      {WEEKS.flatMap(w => w.seances).filter(s => notes[s.id]).length === 0 ? (
        <div className="glass rounded-2xl p-6 text-center text-slate-500 text-sm">
          Tes observations apparaîtront ici après chaque séance.
        </div>
      ) : (
        WEEKS.flatMap(w => w.seances).filter(s => notes[s.id]).map(s => {
          const week = WEEKS.find(w => w.seances.includes(s));
          return (
            <div key={s.id} className="glass rounded-xl p-4">
              <div className="flex items-center justify-between mb-2">
                <div>
                  <span className="text-xs text-slate-500">S{week.num} · </span>
                  <span className="text-sm font-medium text-white">{s.titre}</span>
                </div>
                <span className="text-sky-400 font-bold text-sm">{scores[s.id]}/10</span>
              </div>
              <p className="text-slate-400 text-sm italic">"{notes[s.id]}"</p>
            </div>
          );
        })
      )}
    </div>
  </div>

  <div className="fixed bottom-0 left-0 right-0 bg-slate-950 border-t border-slate-800 px-6 py-3">
    <div className="max-w-2xl mx-auto flex gap-2">
      {[{ id: "home", label: "Accueil", icon: "🏠" }, { id: "programme", label: "Programme", icon: "📅" }, { id: "journal", label: "Journal", icon: "📊" }, { id: "profil", label: "Profil", icon: "👤" }].map(tab => (
        <button key={tab.id} onClick={() => setView(tab.id)} className={`nav-btn flex-1 py-2 rounded-xl text-center ${view === tab.id ? "active" : ""}`}>
          <div className="text-lg">{tab.icon}</div>
          <div className="text-xs text-slate-500">{tab.label}</div>
        </button>
      ))}
    </div>
  </div>
</div>
```

);

// ─── PROFIL ─────────────────────────────────────────────────────
if (view === “profil”) return (
<div className=“min-h-screen bg-slate-950 text-white” style={{ fontFamily: “‘DM Sans’, sans-serif” }}>
<style>{`@import url('https://fonts.googleapis.com/css2?family=DM+Sans:ital,wght@0,300;0,400;0,500;0,700;1,300&family=Playfair+Display:ital,wght@0,700;1,600&display=swap'); * { box-sizing: border-box; } .glass { background: rgba(255,255,255,0.04); border: 1px solid rgba(255,255,255,0.08); } .profil-card { transition: all 0.3s; cursor: pointer; border: 2px solid transparent; } .profil-card:hover { border-color: rgba(14,165,233,0.4); } .profil-card.sel { border-color: #0EA5E9; background: rgba(14,165,233,0.08); } .nav-btn { transition: all 0.2s; } .nav-btn.active { background: rgba(14,165,233,0.2); color: #38BDF8; }`}</style>

```
  <div className="max-w-2xl mx-auto px-6 pt-10 pb-24">
    <p className="text-slate-500 text-xs uppercase tracking-widest mb-1">Le Grand Bain</p>
    <h2 className="text-2xl font-bold mb-2" style={{ fontFamily: "'Playfair Display', serif" }}>Ton profil</h2>
    <p className="text-slate-400 text-sm mb-8">Le programme s'adapte à ton niveau de départ. Identifie-toi pour personnaliser chaque séance.</p>

    <div className="space-y-4 mb-8">
      {[
        {
          id: "Anxieux",
          emoji: "😰",
          title: "Nageur anxieux",
          desc: "Tu sais nager techniquement mais tu paniques en eau profonde, tu t'épuises rapidement, ou tu perds confiance dès que les pieds ne touchent plus le fond.",
          badge: "Profil le plus fréquent",
          color: "rose",
        },
        {
          id: "Rouillé",
          emoji: "🔧",
          title: "Nageur rouillé",
          desc: "Tu as appris à nager enfant mais tu n'as pas nagé depuis des années. Tu as les bases mais tu as perdu la confiance et les automatismes.",
          badge: "Reprise progressive",
          color: "amber",
        },
        {
          id: "Non-nageur",
          emoji: "🌱",
          title: "Non-nageur",
          desc: "Tu n'as jamais vraiment appris à nager, ou tes apprentissages passés n'ont pas fonctionné. Tu pars de zéro — et c'est parfaitement normal.",
          badge: "Départ de zéro",
          color: "teal",
        },
      ].map(p => (
        <div key={p.id} className={`glass profil-card rounded-2xl p-5`}>
          <div className="flex items-start gap-4">
            <div className="text-3xl">{p.emoji}</div>
            <div className="flex-1">
              <div className="flex items-center gap-2 mb-1">
                <span className="font-semibold text-white">{p.title}</span>
                <span className={`text-xs px-2 py-0.5 rounded-full bg-${p.color}-500/10 text-${p.color}-400 border border-${p.color}-500/20`}>{p.badge}</span>
              </div>
              <p className="text-slate-400 text-sm leading-relaxed">{p.desc}</p>
            </div>
          </div>
        </div>
      ))}
    </div>

    {/* Coach Card */}
    <div className="glass rounded-2xl p-5 mb-5">
      <div className="flex items-center gap-4">
        <div className="w-16 h-16 rounded-2xl bg-gradient-to-br from-sky-500 to-cyan-600 flex items-center justify-center text-2xl flex-shrink-0">🏊</div>
        <div>
          <div className="text-xs text-slate-500 mb-0.5">Ton coach</div>
          <div className="font-semibold text-white">Maître-Nageur Sauveteur</div>
          <div className="text-slate-400 text-sm">Diplômé d'État · Spécialiste confiance aquatique</div>
        </div>
      </div>
      <div className="mt-4 pt-4 border-t border-slate-800">
        <p className="text-slate-400 text-sm leading-relaxed italic">
          "Ce programme ne t'apprend pas à nager. Il t'apprend pourquoi tu paniques — et comment ne plus jamais le faire."
        </p>
      </div>
    </div>

    {/* Garantie */}
    <div className="rounded-2xl p-5" style={{ background: "linear-gradient(135deg, rgba(14,165,233,0.1), rgba(6,182,212,0.05))", border: "1px solid rgba(14,165,233,0.2)" }}>
      <div className="flex items-start gap-3">
        <div className="text-2xl">🛡️</div>
        <div>
          <div className="font-semibold text-white mb-1">Garantie 30 jours</div>
          <p className="text-slate-400 text-sm leading-relaxed">Suis les 3 premières semaines. Si tu ne constates aucune progression sur ton score de confiance, remboursement intégral sans question.</p>
        </div>
      </div>
    </div>
  </div>

  <div className="fixed bottom-0 left-0 right-0 bg-slate-950 border-t border-slate-800 px-6 py-3">
    <div className="max-w-2xl mx-auto flex gap-2">
      {[{ id: "home", label: "Accueil", icon: "🏠" }, { id: "programme", label: "Programme", icon: "📅" }, { id: "journal", label: "Journal", icon: "📊" }, { id: "profil", label: "Profil", icon: "👤" }].map(tab => (
        <button key={tab.id} onClick={() => setView(tab.id)} className={`nav-btn flex-1 py-2 rounded-xl text-center ${view === tab.id ? "active" : ""}`}>
          <div className="text-lg">{tab.icon}</div>
          <div className="text-xs text-slate-500">{tab.label}</div>
        </button>
      ))}
    </div>
  </div>
</div>
```

);

return null;
}
