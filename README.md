<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>WA Logo Link Generator</title>
<style>
  body { font-family: Arial, sans-serif; text-align: center; padding: 50px; background: #f2f2f2; }
  h1 { color: #075e54; }
  input { padding: 10px; width: 60%; margin-bottom: 10px; border-radius: 5px; border: 1px solid #ccc; }
  button { padding: 10px 20px; margin-top: 10px; border-radius: 5px; border: none; background: #25D366; color: white; cursor: pointer; }
  button:hover { background: #128C7E; }
  #resultContainer { margin-top: 20px; display: none; }
  #resultLink { display: block; padding: 10px; background: white; border: 1px solid #ccc; border-radius: 5px; text-decoration: none; color: #075e54; word-break: break-all; }
  #copyBtn { margin-top: 10px; padding: 8px 15px; border-radius: 5px; border: none; background: #128C7E; color: white; cursor: pointer; }
  #copyBtn:hover { background: #075E54; }
</style>
</head>
<body>

<h1>WA Logo Link Generator</h1>
<p>Masukkan link tujuan:</p>
<input type="text" id="targetUrl" placeholder="https://example.com">
<br>
<button onclick="generateLink()">Buat Link dengan Logo WA</button>

<div id="resultContainer">
    <a id="resultLink" href="#" target="_blank">Link Hasil Shorten</a>
    <button id="copyBtn" onclick="copyLink()">Copy Link</button>
</div>

<script>
function generateLink() {
    const target = document.getElementById('targetUrl').value;
    if (!target) { alert("Masukkan link dulu!"); return; }

    // Buat redirect.html dinamis dengan OG WA
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

    // Simpan sebagai blob (sementara)
    const blob = new Blob([redirectHTML], { type: "text/html" });
    const url = URL.createObjectURL(blob);

    // Tampilkan link hasil shorten
    const resultContainer = document.getElementById('resultContainer');
    const resultLink = document.getElementById('resultLink');
    const copyBtn = document.getElementById('copyBtn');

    resultLink.href = url;
    resultLink.textContent = url;
    copyBtn.dataset.link = url;

    resultContainer.style.display = "block";
}

function copyLink() {
    const copyBtn = document.getElementById('copyBtn');
    const link = copyBtn.dataset.link;
    navigator.clipboard.writeText(link)
      .then(() => alert("Link berhasil disalin!"))
      .catch(() => alert("Gagal menyalin link."));
}
</script>

</body>
</html>
