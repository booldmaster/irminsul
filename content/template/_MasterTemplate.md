<%*
// --- CONSTANTS & CONFIG --- 
const folderOpini = "Opini";
const folderAnekdot = "Anekdot Besar";
const folderArtikel = "Artikel Kecil";


// Region List for Suggestion

  

// --- Step 1a: Type Defining ---
const type = await tp.system.suggester(
  ["Opini", "Anekdot Besar", "Artikel Kecil"],
  ["opini", "anekdot", "artikel"],
  false,
  "Pilih jenis notes yang ingin dibuat:"
);

if (!type) { new Notice("Script cancelled."); return; }

// --- Step 1b: Tags Defining ---
const tags = await tp.system.prompt("Tags (pisahkan dengan koma)", "", false) || "";

// --- Step 1c: Aliases Defining ---
const aliases = await tp.system.prompt("Aliases (pisahkan dengan koma)", "", false) || "";

// --- Step 1d: Topic Defining (Sanitized) --- 
const topicsInput = await tp.system.prompt("Masukkan topik (pisahkan dengan koma):", "", false);

const topic = topicsInput
  ? topicsInput.split(",")
      .map(t => t.trim())
      .filter(t => t.length > 0)
      .map(t => `"[[${t.replace(/[\\/:*?"<>|\[\]#^]/g, "")}]]"`)
      .join(", ")
  : "";

// --- Step 2: Title & Renaming ---
let title = await tp.system.prompt("Masukkan judul:", tp.file.title);
await tp.file.rename(title);

// --- Step 3: Move Logic ---
if (type === "opini") {
  await tp.file.move(folderOpini + "/" + title);
} else if (type === "anekdot") {
  await tp.file.move(folderAnekdot + "/" + title);
} else if (type === "artikel") {
  await tp.file.move(folderArtikel + "/" + title);
}

// --- Step 4: Conditional Publish Boolean Assign ---
let publishProp = "";
if (type) {
	const isPublish = await tp.system.suggester(["Ya (Publish)", "Tidak"], [true, false], false, "Publish catatan ini?");
	if (isPublish !== undefined) {
	publishProp = `\npublish: ${isPublish}`;
	}
}

_%>
---
type: <% type %> 
tags: [<% tags %>]
aliases: [<% aliases %>]<% publishProp %>
topic: [<% topic %>]
---