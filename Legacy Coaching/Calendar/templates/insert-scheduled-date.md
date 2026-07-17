<%*
const input = await tp.system.prompt("Scheduled date (YYYY-MM-DD):", tp.date.now("YYYY-MM-DD"));
const isValid = input && /^\d{4}-\d{2}-\d{2}$/.test(input);

if (!isValid) {
  new Notice("Not a valid YYYY-MM-DD date — nothing inserted. Run the command again.");
} else {
  tR += `**Scheduled for** [[${input}]]`;
  new Notice(`Linked to ${input}. Now set "publish_date: ${input}" in this note's frontmatter so Dataview and the daily note pick it up — the wikilink is the visual/graph layer, frontmatter is the single source of truth.`);
}
-%>
