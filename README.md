
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Safira Áudios</title>
<style>
* { box-sizing: border-box; }
body {
  margin: 0;
  padding: 25px 18px;
  background: #120b1c;
  color: white;
  font-family: Arial, sans-serif;
  text-align: center;
}
h1 { color: #d8a5ff; }
p { color: #d6c9df; }
.card {
  max-width: 430px;
  margin: 20px auto;
  padding: 22px;
  background: #241632;
  border-radius: 18px;
}
label {
  display: block;
  margin: 18px 0 8px;
  color: #e5caff;
  font-weight: bold;
}
input {
  width: 100%;
  padding: 12px;
  background: #352344;
  color: white;
  border-radius: 10px;
}
button {
  width: 100%;
  padding: 14px;
  margin-top: 13px;
  border: 0;
  border-radius: 12px;
  background: #bd83ed;
  color: #1b1025;
  font-size: 16px;
  font-weight: bold;
}
button:disabled { opacity: .4; }
img {
  display: none;
  width: 170px;
  height: 170px;
  object-fit: cover;
  border-radius: 15px;
  margin: 18px auto;
}
audio { width: 100%; margin-top: 15px; }
#status { margin-top: 15px; font-size: 14px; }
</style>
</head>
<body>

<h1>💜 Safira Áudios</h1>
<p>Escolha uma capa e um áudio para compartilhar.</p>

<div class="card">
  <label for="cover">🖼️ Escolher capa</label>
  <input id="cover" type="file" accept="image/*">

  <img id="preview" alt="Capa do áudio">

  <label for="audio">🎵 Escolher áudio</label>
  <input id="audio" type="file" accept="audio/*">

  <audio id="player" controls hidden></audio>

  <button id="download" disabled>⬇️ Baixar áudio</button>
  <button id="share" disabled>📲 Compartilhar áudio</button>
  <button id="clear">🗑️ Limpar tudo</button>

  <div id="status"></div>
</div>

<script>
const cover = document.getElementById('cover');
const audio = document.getElementById('audio');
const preview = document.getElementById('preview');
const player = document.getElementById('player');
const download = document.getElementById('download');
const share = document.getElementById('share');
const status = document.getElementById('status');

let audioURL = null;
let imageURL = null;

cover.addEventListener('change', () => {
  if (imageURL) URL.revokeObjectURL(imageURL);
  const file = cover.files[0];
  if (!file) return;
  imageURL = URL.createObjectURL(file);
  preview.src = imageURL;
  preview.style.display = 'block';
});

audio.addEventListener('change', () => {
  if (audioURL) URL.revokeObjectURL(audioURL);
  const file = audio.files[0];
  if (!file) return;
  audioURL = URL.createObjectURL(file);
  player.src = audioURL;
  player.hidden = false;
  download.disabled = false;
  share.disabled = false;
  status.textContent = 'Áudio pronto para compartilhar! 💜';
});

download.addEventListener('click', () => {
  const file = audio.files[0];
  if (!file) return;
  const link = document.createElement('a');
  link.href = audioURL;
  link.download = file.name;
  link.click();
});

share.addEventListener('click', async () => {
  const file = audio.files[0];
  if (!file) return;

  try {
    if (navigator.canShare && navigator.canShare({files: [file]})) {
      await navigator.share({
        files: [file],
        title: 'Meu áudio',
        text: 'Olha esse áudio! 💜'
      });
    } else {
      status.textContent = 'Seu navegador não permite compartilhar o arquivo direto. Baixe o áudio e anexe no WhatsApp.';
    }
  } catch (error) {
    if (error.name !== 'AbortError') {
      status.textContent = 'Não foi possível compartilhar. Tente baixar o áudio.';
    }
  }
});

document.getElementById('clear').addEventListener('click', () => {
  if (audioURL) URL.revokeObjectURL(audioURL);
  if (imageURL) URL.revokeObjectURL(imageURL);
  audio.value = '';
  cover.value = '';
  audioURL = null;
  imageURL = null;
  player.pause();
  player.removeAttribute('src');
  player.hidden = true;
  preview.removeAttribute('src');
  preview.style.display = 'none';
  download.disabled = true;
  share.disabled = true;
  status.textContent = 'Tudo limpo! 💜';
});
</script>
</body>
</html>
