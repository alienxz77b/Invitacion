// script.js — carga mesas.json, dibuja mesas en el SVG y marca la mesa solicitada en URL (?mesa=...)


const rootStyle = getComputedStyle(document.documentElement);
const ACCENT = (rootStyle.getPropertyValue('--accent') || '#ff66b2').trim();
const TABLE_FILL = (rootStyle.getPropertyValue('--table-fill') || '#fff3f8').trim();


document.addEventListener('DOMContentLoaded', () => {
const params = new URLSearchParams(window.location.search);
const code = params.get('mesa') || params.get('code') || params.get('id');
const statusEl = document.getElementById('status');
const codeBadge = document.getElementById('codeBadge');


fetch('./mesas.json')
.then(resp => {
if(!resp.ok) throw new Error('No se pudo cargar mesas.json (HTTP ' + resp.status + ')');
return resp.json();
})
.then(mesas => {
renderTables(mesas);


if(!code){
statusEl.innerHTML = 'No se indicó mesa en la URL. Usa <code>?mesa=101</code> para probar.';
return;
}


codeBadge.textContent = code;
const mesa = mesas[code];
if(!mesa){
statusEl.innerHTML = `<span class="notfound">Mesa ${code} no encontrada.</span>`;
return;
}


statusEl.textContent = `Mesa ${code} — Zona: ${mesa.zona || '-'} `;
highlightTable(code);
})
.catch(err => {
statusEl.textContent = 'Error cargando datos: ' + err.message;
console.error(err);
});
});


function renderTables(mesas){
const svg = document.getElementById('svgmap');
const group = document.createElementNS('http://www.w3.org/2000/svg','g');
group.setAttribute('id','mesas');


Object.entries(mesas).forEach(([code, info]) => {
const g = document.createElementNS('http://www.w3.org/2000/svg','g');
g.setAttribute('class','table');
g.setAttribute('data-code', code);
g.setAttribute('transform', `translate(${info.x}, ${info.y})`);


if(info.shape && info.shape.toLowerCase() === 'rect'){
const rect = document.createElementNS('http://www.w3.org/2000/svg','rect');
rect.setAttribute('x', -36);
rect.setAttribute('y', -22);
rect.setAttribute('width', 72);
rect.setAttribute('height', 44);
rect.setAttribute('rx', 8);
rect.setAttribute('fill', TABLE_FILL);
rect.setAttribute('stroke', ACCENT);
rect.setAttribute('stroke-width', '3');
g.appendChild(rect);
}
