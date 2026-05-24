<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Exercices de Statistiques</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { font-family: 'Segoe UI', Arial, sans-serif; background: #f0f0fa; color: #222; min-height: 100vh; padding: 20px; }
  h1 { text-align: center; color: #3333aa; font-size: 22px; margin-bottom: 6px; }
  .subtitle { text-align: center; color: #666; font-size: 13px; margin-bottom: 20px; font-style: italic; }
  .nav { display: flex; gap: 6px; flex-wrap: wrap; justify-content: center; margin-bottom: 20px; }
  .nav button {
    font-size: 13px; padding: 6px 14px;
    border-radius: 8px; border: 1.5px solid #b0b0e0;
    background: #fff; color: #555; cursor: pointer; transition: all 0.15s;
  }
  .nav button.active { background: #4b4bcf; color: #fff; border-color: #4b4bcf; font-weight: 600; }
  .nav button:hover:not(.active) { background: #ededfc; }
  .card { background: #fff; border-radius: 12px; box-shadow: 0 2px 12px rgba(80,80,180,0.10); padding: 24px; max-width: 680px; margin: 0 auto; }
  .exo { display: none; }
  .exo.visible { display: block; }
  .exo-title { font-size: 16px; font-weight: 700; color: #3333aa; margin-bottom: 4px; }
  .exo-serie { font-size: 13px; color: #666; font-style: italic; margin-bottom: 18px; }
  .section-label { font-size: 11px; font-weight: 700; color: #7b7be8; text-transform: uppercase; letter-spacing: 0.07em; margin: 16px 0 8px; }
  table { width: 100%; border-collapse: collapse; margin-bottom: 4px; }
  th { background: #4b4bcf; color: #fff; font-size: 13px; font-weight: 600; padding: 8px 12px; text-align: center; }
  th.left { text-align: left; }
  td { border: 1px solid #d0d0ec; font-size: 13px; padding: 7px 12px; text-align: center; }
  td.label { text-align: left; color: #333; }
  td.hint { color: #5555cc; font-style: italic; font-size: 12px; text-align: left; border: none; padding-top: 6px; }
  td.row-label { text-align: left; font-weight: 600; color: #4b4bcf; background: #ededfc; }
  tr:nth-child(even) td { background: #f7f7fd; }
  tr:nth-child(even) td.row-label { background: #e4e4f8; }
  .input-cell input {
    width: 100%; padding: 5px 8px;
    border-radius: 6px; border: 1.5px solid #c0c0e0;
    font-size: 13px; text-align: center; background: #fff; color: #222;
    outline: none; transition: border-color 0.2s;
  }
  .input-cell input:focus { border-color: #4b4bcf; }
  .input-cell input.correct { border-color: #0f7; background: #e8fdf3; color: #0a5c3a; font-weight: 600; }
  .input-cell input.wrong { border-color: #e05; background: #fdeaea; color: #7a1020; }
  .actions { display: flex; gap: 10px; margin-top: 18px; }
  .btn-check {
    padding: 8px 22px; border-radius: 8px;
    border: none; background: #4b4bcf; color: #fff;
    cursor: pointer; font-size: 13px; font-weight: 600;
    transition: background 0.15s;
  }
  .btn-check:hover { background: #3333aa; }
  .btn-reset {
    padding: 8px 16px; border-radius: 8px;
    border: 1.5px solid #c0c0e0; background: #fff; color: #666;
    cursor: pointer; font-size: 13px; transition: background 0.15s;
  }
  .btn-reset:hover { background: #f0f0fa; }
  .score { font-size: 13px; color: #666; margin-top: 10px; min-height: 20px; line-height: 1.6; }
  .score .ok { color: #0a5c3a; font-weight: 700; }
  .score .nok { color: #7a1020; font-weight: 700; }
  .progress { text-align: center; font-size: 12px; color: #888; margin-top: 18px; }
  .progress-bar { height: 6px; background: #e0e0f0; border-radius: 3px; margin: 6px 0; overflow: hidden; }
  .progress-fill { height: 100%; background: #4b4bcf; border-radius: 3px; transition: width 0.4s; }
</style>
</head>
<body>
 
<h1>Exercices de Statistiques</h1>
<p class="subtitle">Identifier les variables et les effectifs dans des tableaux statistiques</p>
 
<div class="nav" id="nav"></div>
 
<div class="card">
  <div id="exos"></div>
  <div class="progress">
    <div id="progress-text">0 / 10 exercices réussis</div>
    <div class="progress-bar"><div class="progress-fill" id="progress-fill" style="width:0%"></div></div>
  </div>
</div>
 
<script>
const exercices = [
  {
    titre: "Exercice 1 — Taille des élèves d'une classe",
    serie: "Taille des élèves (en cm) — 25 élèves",
    col1: "Taille (cm)", col2: "Effectif",
    valeurs: "Valeurs centrales : 152 – 157 – 162 – 167 – 172",
    lignes: ["[150 ; 155[", "[155 ; 160[", "[160 ; 165[", "[165 ; 170[", "[170 ; 175["],
    repValeurs: "Taille (cm)", repEffectifs: "Effectif"
  },
  {
    titre: "Exercice 2 — Temps de trajet domicile-travail",
    serie: "Temps de trajet (en min) — 40 personnes",
    col1: "Durée (min)", col2: "Nombre de personnes",
    valeurs: "Valeurs centrales : 5 – 15 – 25 – 35 – 45",
    lignes: ["[0 ; 10[", "[10 ; 20[", "[20 ; 30[", "[30 ; 40[", "[40 ; 50["],
    repValeurs: "Durée (min)", repEffectifs: "Nombre de personnes"
  },
  {
    titre: "Exercice 3 — Prix de vente de maisons",
    serie: "Prix de vente (en milliers d'€) — 20 maisons",
    col1: "Prix (k€)", col2: "Nombre de maisons",
    valeurs: "Valeurs centrales : 125 – 175 – 225 – 275 – 325",
    lignes: ["[100 ; 150[", "[150 ; 200[", "[200 ; 250[", "[250 ; 300[", "[300 ; 350["],
    repValeurs: "Prix (k€)", repEffectifs: "Nombre de maisons"
  },
  {
    titre: "Exercice 4 — Consommation journalière d'eau",
    serie: "Consommation d'eau (en litres/jour) — 30 foyers",
    col1: "Consommation (L)", col2: "Nombre de foyers",
    valeurs: "Valeurs centrales : 55 – 85 – 115 – 145 – 175",
    lignes: ["[40 ; 70[", "[70 ; 100[", "[100 ; 130[", "[130 ; 160[", "[160 ; 190["],
    repValeurs: "Consommation (L)", repEffectifs: "Nombre de foyers"
  },
  {
    titre: "Exercice 5 — Masse des bagages en avion",
    serie: "Masse des bagages (en kg) — 50 passagers",
    col1: "Masse (kg)", col2: "Nombre de passagers",
    valeurs: "Valeurs centrales : 5 – 10 – 15 – 20 – 25",
    lignes: ["[0 ; 7,5[", "[7,5 ; 12,5[", "[12,5 ; 17,5[", "[17,5 ; 22,5[", "[22,5 ; 27,5["],
    repValeurs: "Masse (kg)", repEffectifs: "Nombre de passagers"
  },
  {
    titre: "Exercice 6 — Heures de sommeil par nuit",
    serie: "Heures de sommeil par nuit — 35 personnes",
    col1: "Durée (h)", col2: "Effectif",
    valeurs: "Valeurs centrales : 5,5 – 6,5 – 7,5 – 8,5 – 9,5",
    lignes: ["[5 ; 6[", "[6 ; 7[", "[7 ; 8[", "[8 ; 9[", "[9 ; 10["],
    repValeurs: "Durée (h)", repEffectifs: "Effectif"
  },
  {
    titre: "Exercice 7 — Scores à un jeu vidéo",
    serie: "Scores obtenus (en points) — 20 joueurs",
    col1: "Score (pts)", col2: "Nombre de joueurs",
    valeurs: "Valeurs centrales : 250 – 750 – 1250 – 1750 – 2250",
    lignes: ["[0 ; 500[", "[500 ; 1000[", "[1000 ; 1500[", "[1500 ; 2000[", "[2000 ; 2500["],
    repValeurs: "Score (pts)", repEffectifs: "Nombre de joueurs"
  },
  {
    titre: "Exercice 8 — Température maximale en été",
    serie: "Températures max (en °C) — 60 jours observés",
    col1: "Température (°C)", col2: "Nombre de jours",
    valeurs: "Valeurs centrales : 22 – 26 – 30 – 34 – 38",
    lignes: ["[20 ; 24[", "[24 ; 28[", "[28 ; 32[", "[32 ; 36[", "[36 ; 40["],
    repValeurs: "Température (°C)", repEffectifs: "Nombre de jours"
  },
  {
    titre: "Exercice 9 — Dépenses mensuelles en courses",
    serie: "Dépenses en courses (en €) — 45 familles",
    col1: "Dépenses (€)", col2: "Nombre de familles",
    valeurs: "Valeurs centrales : 150 – 250 – 350 – 450 – 550",
    lignes: ["[100 ; 200[", "[200 ; 300[", "[300 ; 400[", "[400 ; 500[", "[500 ; 600["],
    repValeurs: "Dépenses (€)", repEffectifs: "Nombre de familles"
  },
  {
    titre: "Exercice 10 — Distance parcourue à vélo",
    serie: "Distance quotidienne à vélo (en km) — 28 cyclistes",
    col1: "Distance (km)", col2: "Nombre de cyclistes",
    valeurs: "Valeurs centrales : 3 – 8 – 13 – 18 – 23",
    lignes: ["[0 ; 5[", "[5 ; 10[", "[10 ; 15[", "[15 ; 20[", "[20 ; 25["],
    repValeurs: "Distance (km)", repEffectifs: "Nombre de cyclistes"
  }
];
 
const scores = new Array(exercices.length).fill(false);
const nav = document.getElementById('nav');
const exosDiv = document.getElementById('exos');
 
function updateProgress() {
  const total = scores.filter(Boolean).length;
  document.getElementById('progress-text').textContent = total + ' / ' + exercices.length + ' exercices réussis';
  document.getElementById('progress-fill').style.width = (total / exercices.length * 100) + '%';
}
 
function normalize(s) {
  return s.trim().toLowerCase().replace(/[\s\-_]/g,'').replace(/[éèê]/g,'e').replace(/[àâ]/g,'a').replace(/[ùû]/g,'u').replace(/[îï]/g,'i').replace(/[ôö]/g,'o').replace(/°/g,'').replace(/\(/g,'').replace(/\)/g,'');
}
 
exercices.forEach((ex, i) => {
  const btn = document.createElement('button');
  btn.textContent = 'Ex. ' + (i+1);
  btn.id = 'btn-' + i;
  if (i === 0) btn.classList.add('active');
  btn.onclick = () => {
    document.querySelectorAll('.nav button').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    document.querySelectorAll('.exo').forEach(e => e.classList.remove('visible'));
    document.getElementById('exo-' + i).classList.add('visible');
  };
  nav.appendChild(btn);
 
  const div = document.createElement('div');
  div.className = 'exo' + (i === 0 ? ' visible' : '');
  div.id = 'exo-' + i;
 
  div.innerHTML = `
    <div class="exo-title">${ex.titre}</div>
    <div class="exo-serie">${ex.serie}</div>
 
    <div class="section-label">Tableau de données</div>
    <table>
      <thead><tr><th class="left">${ex.col1}</th><th>${ex.col2}</th></tr></thead>
      <tbody>
        ${ex.lignes.map(l => `<tr><td class="label">${l}</td><td></td></tr>`).join('')}
      </tbody>
    </table>
 
    <div class="section-label">Tableau de correspondance — à compléter</div>
    <table>
      <thead><tr><th class="left"></th><th>Valeurs</th><th>Effectifs</th></tr></thead>
      <tbody>
        <tr>
          <td class="row-label">${ex.serie.split('—')[0].trim()}</td>
          <td class="input-cell"><input type="text" id="v-${i}" placeholder="…" autocomplete="off" spellcheck="false"></td>
          <td class="input-cell"><input type="text" id="e-${i}" placeholder="…" autocomplete="off" spellcheck="false"></td>
        </tr>
        <tr><td class="hint" colspan="3">Indice — ${ex.valeurs}</td></tr>
      </tbody>
    </table>
 
    <div class="actions">
      <button class="btn-check" onclick="check(${i})">Vérifier ✓</button>
      <button class="btn-reset" onclick="reset(${i})">Recommencer</button>
    </div>
    <div class="score" id="score-${i}"></div>
  `;
  exosDiv.appendChild(div);
});
 
function check(i) {
  const ex = exercices[i];
  const vIn = document.getElementById('v-' + i);
  const eIn = document.getElementById('e-' + i);
  const scoreEl = document.getElementById('score-' + i);
  const vOk = normalize(vIn.value) === normalize(ex.repValeurs);
  const eOk = normalize(eIn.value) === normalize(ex.repEffectifs);
  vIn.className = vIn.value ? (vOk ? 'correct' : 'wrong') : '';
  eIn.className = eIn.value ? (eOk ? 'correct' : 'wrong') : '';
  const total = (vOk ? 1 : 0) + (eOk ? 1 : 0);
  scores[i] = total === 2;
  updateProgress();
  if (total === 2) {
    scoreEl.innerHTML = '<span class="ok">✓ Bravo, les deux réponses sont correctes !</span>';
    document.getElementById('btn-' + i).style.background = '#0f7';
    document.getElementById('btn-' + i).style.color = '#0a5c3a';
    document.getElementById('btn-' + i).style.borderColor = '#0f7';
  } else if (total === 1) {
    scoreEl.innerHTML = '<span class="nok">1 réponse sur 2 correcte.</span><br>'
      + (!vOk ? 'Valeurs attendues : <strong>' + ex.repValeurs + '</strong><br>' : '')
      + (!eOk ? 'Effectifs attendus : <strong>' + ex.repEffectifs + '</strong>' : '');
  } else {
    scoreEl.innerHTML = '<span class="nok">Aucune réponse correcte.</span><br>'
      + 'Valeurs : <strong>' + ex.repValeurs + '</strong> — Effectifs : <strong>' + ex.repEffectifs + '</strong>';
  }
}
 
function reset(i) {
  ['v-','e-'].forEach(p => {
    const el = document.getElementById(p + i);
    el.value = ''; el.className = '';
  });
  scores[i] = false;
  document.getElementById('score-' + i).innerHTML = '';
  const btn = document.getElementById('btn-' + i);
  btn.style.background = ''; btn.style.color = ''; btn.style.borderColor = '';
  updateProgress();
}
</script>
</body>
</html>
