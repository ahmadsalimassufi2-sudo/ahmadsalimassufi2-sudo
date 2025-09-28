<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>WA Logo Link Generator Multi</title>
<style>
  body { font-family: Arial, sans-serif; text-align: center; padding: 50px; background: #f2f2f2; }
  h1 { color: #075e54; }
  textarea { padding: 10px; width: 80%; height: 120px; margin-bottom: 10px; border-radius: 5px; border: 1px solid #ccc; resize: vertical; }
  button { padding: 10px 20px; margin-top: 10px; border-radius: 5px; border: none; background: #25D366; color: white; cursor: pointer; }
  button:hover { background: #128C7E; }
  .resultContainer { margin-top: 20px; display: flex; flex-direction: column; align-items: center; }
  .linkBox { background: white; padding: 10px; margin: 5px; border-radius: 5px; width: 80%; display: flex; justify-content: space-between; align-items: center; border: 1px solid #ccc; }
  .linkBox a { word-break: break-all; color: #075e54; text-decoration: none; flex: 1; margin-right: 10px; }
  .copyBtn { padding: 5px 10px; border-radius: 5px; border: none; background: #128C7E; color: white; cursor: pointer; }
  .copyBtn:hover { background: #075E54; }
  #copyAllBtn { margin-top: 10px; background: #075E54; }
  #copyAllBtn:hover { background: #128C7E; }
</style>
</head>
<body>

<h1>WA Logo Link Generator Multi</h1>
<p>Masukkan satu atau banyak link (satu link per baris):</p>
<textarea id="targetUrls" placeholder="https://example.com&#10;https://google.com"></textarea>
<br>
<button onclick="generateLinks()">Buat Semua Link dengan Logo WA</button>

<div class="resultContainer" id="resultContainer"></div>
<button id="copyAllBtn" style="display:none;" onclick="copyAllLinks()">Copy Semua Link</button>

<script>
function generateLinks() {
    const textarea = document.getElementById('targetUrls');
    const lines = textarea.value.split('\n').map(l => l.trim()).filter(l => l !== '');
    const container = document.getElementById('resultContainer');
    container.innerHTML = ''; // clear previous
    if (lines.length === 0) { alert("Masukkan minimal satu link!"); return; }

    lines.forEach(target => {
        const redirectHTML = `
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<title>Redirecting...</title>
<meta property="og:title" content="Klik link ini!">
<meta property="og:description" content="Link keren dengan logo WhatsApp">
<meta property="og:image" content="https://i.ibb.co/0jqHpnp/whatsapp-logo.png">
<meta property="og:url" content="${target}">
<meta http-equiv="refresh" content="0;url=${target}">
</head>
<body>
<p>Redirecting… jika tidak otomatis, <a href="${target}">klik di sini</a>.</p>
</body>
</html>
`;

        const blob = new Blob([redirectHTML], { type: "text/html" });
        const url = URL.createObjectURL(blob);

        // Buat tampilan link box
        const box = document.createElement('div');
        box.className = 'linkBox';

        const linkEl = document.createElement('a');
        linkEl.href = url;
        linkEl.target = '_blank';
        linkEl.textContent = url;

        const copyBtn = document.createElement('button');
        copyBtn.className = 'copyBtn';
        copyBtn.textContent = 'Copy';
        copyBtn.onclick = () => {
            navigator.clipboard.writeText(url).then(() => alert("Link berhasil disalin!"));
        };

        box.appendChild(linkEl);
        box.appendChild(copyBtn);
        container.appendChild(box);
    });

    // Tampilkan tombol copy semua
    document.getElementById('copyAllBtn').style.display = 'inline-block';
}

function copyAllLinks() {
    const links = Array.from(document.querySelectorAll('.linkBox a')).map(a => a.href).join('\n');
    navigator.clipboard.writeText(links)
      .then(() => alert("Semua link berhasil disalin!"))
      .catch(() => alert("Gagal menyalin link."));
}
</script>

</body>
</html>
