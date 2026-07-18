<%*
// --- CONSTANTS & CONFIG --- 
const folderOpini = "Opini";
const folderAnekdot = "Anekdot Besar";
const folderArtikel = "Artikel Kecil";


// Region List for Suggestion

  

// --- Step 1a: Type Defining---

const type = await tp.system.suggester(
  ["Opini", "Anekdot Besar", "Artikel Kecil"],
  ["opini", "anekdot", "artikel"],
  false,
  "Pilih jenis notes yang ingin dibuat:"
);

if (!type) { new Notice("Script cancelled."); return; }

// --- Step 1b: Tags Defining---
const tags = await tp.system.prompt("Tags (comma separated)", "", false) || "";
// --- Step 1c: Aliases Defining---
const aliases = await tp.system.prompt("Aliases (comma separated)", "", false) || "";
// --- Step 2: Gather Metadata  ---


// --- Step x: Title & Renaming ---

let title = await tp.system.prompt("Masukkan judul:", tp.file.title);
await tp.file.rename(title);

// --- Step y: Move to Folder ---
if (type === "opini") {
  
  // Move to Opini folder
  await tp.file.move(folderOpini + "/" + title);
} else if (type === "anekdot") {
  
  // Move to Anekdot Besar folder
  await tp.file.move(folderAnekdot + "/" + title);
} else if (type === "artikel") {

  // Move to Artikel Kecil folder
  await tp.file.move(folderArtikel + "/" + title);
}
_%>
---
type: <% type %> 
tags: [<% tags %>]
aliases: [<% aliases %>]
entity:
---