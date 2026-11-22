// PIECES AUTO ADJAME - app.js - 2 PRODUITS SEULEMENT
let produits = [];
let panier = JSON.parse(localStorage.getItem('panier') || '[]');
const NUMERO_WHATSAPP = "22555061653"; // <-- CHANGE TON NUMERO ICI

async function chargerProduits() {
  try {
    const reponse = await fetch('produits.json');
    produits = await reponse.json();
    afficherProduits(produits);
    mettreAJourPanier();
  } catch (e) {
    document.getElementById('liste-produits').innerHTML = "<p>Erreur. Vérifie produits.json</p>";
  }
}

function afficherProduits(liste) {
  const zone = document.getElementById('liste-produits');
  if (!zone) return;
  zone.innerHTML = "";
  liste.forEach(p => {
    const div = document.createElement('div');
    div.className = 'produit';
    div.innerHTML = `
      <img src="${p.image}" alt="${p.nom}" onerror="this.src='https://via.placeholder.com/400x300?text=${encodeURIComponent(p.nom)}'">
      <span class="categorie">${p.categorie}</span>
      <h3>${p.nom}</h3>
      <p class="desc">${p.description}</p>
      <p class="stock">✅ ${p.stock}</p>
      <p class="prix">${p.prix.toLocaleString('fr-FR')} FCFA</p>
      <button onclick="ajouterPanier(${p.id})">🛒 Ajouter au Panier</button>
      <button onclick="commanderUnProduit(${p.id})" class="btn-whatsapp">📱 Commander sur WhatsApp</button>
    `;
    zone.appendChild(div);
  });
}

function ajouterPanier(id) {
  const produit = produits.find(p => p.id === id);
  const deja = panier.find(item => item.id === id);
  if (deja) { deja.quantite++; } else { panier.push({ ...produit, quantite: 1 }); }
  sauvegarderPanier();
  alert(produit.nom + " ajouté au panier !");
}

function retirerPanier(id) {
  panier = panier.filter(item => item.id !== id);
  sauvegarderPanier();
}

function changerQuantite(id, delta) {
  const item = panier.find(i => i.id === id);
  if (!item) return;
  item.quantite += delta;
  if (item.quantite <= 0) retirerPanier(id);
  else sauvegarderPanier();
}

function sauvegarderPanier() {
  localStorage.setItem('panier', JSON.stringify(panier));
  mettreAJourPanier();
}

function mettreAJourPanier() {
  const zone = document.getElementById('panier');
  const compteur = document.getElementById('compteur-panier');
  const totalEl = document.getElementById('total-panier');
  if (!zone) return;
  if (panier.length === 0) {
    zone.innerHTML = "<p>Ton panier est vide.</p>";
    if (compteur) compteur.textContent = "0";
    if (totalEl) totalEl.textContent = "0 FCFA";
    return;
  }
  let total = 0;
  let html = "";
  panier.forEach(item => {
    const sousTotal = item.prix * item.quantite;
    total += sousTotal;
    html += `
      <div class="item-panier">
        <b>${item.nom}</b><br>
        ${item.prix.toLocaleString('fr-FR')} FCFA x ${item.quantite} = <b>${sousTotal.toLocaleString('fr-FR')} FCFA</b><br>
        <button onclick="changerQuantite(${item.id}, -1)">-</button>
        <button onclick="changerQuantite(${item.id}, 1)">+</button>
        <button onclick="retirerPanier(${item.id})">❌ Enlever</button>
      </div><hr>
    `;
  });
  zone.innerHTML = html;
  if (compteur) compteur.textContent = panier.reduce((s, i) => s + i.quantite, 0);
  if (totalEl) totalEl.textContent = total.toLocaleString('fr-FR') + " FCFA";
}

function commanderUnProduit(id) {
  const p = produits.find(x => x.id === id);
  const message = `Bonjour Pièces Auto Adjamé, je veux commander :\n- ${p.nom} : ${p.prix.toLocaleString('fr-FR')} FCFA\nC'est encore disponible ?`;
  window.open(`https://wa.me/${NUMERO_WHATSAPP}?text=${encodeURIComponent(message)}`, '_blank');
}

function commanderPanier() {
  if (panier.length === 0) { alert("Ton panier est vide !"); return; }
  let message = "Bonjour Pièces Auto Adjamé, je veux commander :\n";
  let total = 0;
  panier.forEach(item => {
    const sousTotal = item.prix * item.quantite;
    total += sousTotal;
    message += `- ${item.nom} x ${item.quantite} = ${sousTotal.toLocaleString('fr-FR')} FCFA\n`;
  });
  message += `\nTOTAL : ${total.toLocaleString('fr-FR')} FCFA\nC'est disponible ?`;
  window.open(`https://wa.me/${NUMERO_WHATSAPP}?text=${encodeURIComponent(message)}`, '_blank');
}

function viderPanier() { panier = []; sauvegarderPanier(); }

document.addEventListener('DOMContentLoaded', chargerProduits);
