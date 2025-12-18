(function() {
  if (window.myUIActive) return; // prevent duplicates
  window.myUIActive = true;

  // Create overlay panel
  const overlay = document.createElement('div');
  overlay.style.position = 'fixed';
  overlay.style.top = '-400px'; // start hidden above
  overlay.style.right = '50px';
  overlay.style.width = '400px';
  overlay.style.height = '350px';
  overlay.style.backgroundColor = 'black'; // black background
  overlay.style.border = '2px solid #000';
  overlay.style.borderRadius = '15px'; // rounded corners
  overlay.style.boxShadow = '0 0 15px rgba(0,0,0,0.3)';
  overlay.style.zIndex = '9999';
  overlay.style.fontFamily = 'Arial, sans-serif';
  overlay.style.display = 'flex';
  overlay.style.flexDirection = 'column';
  overlay.style.alignItems = 'center';
  overlay.style.padding = '10px';
  overlay.style.position = 'fixed';
  overlay.style.transition = 'top 0.5s ease';

  // Header
  const header = document.createElement('div');
  header.innerText = 'Change Google Wall Paper';
  header.style.position = 'absolute';
  header.style.top = '10px';
  header.style.right = '10px';
  header.style.fontSize = '14px';
  header.style.fontWeight = 'bold';
  header.style.color = 'white'; // Make header text visible on black

  overlay.appendChild(header);

  // Description and drop zone
  const description = document.createElement('div');
  description.innerText = 'Drag and drop an image or video below';
  description.style.fontSize = '12px';
  description.style.marginTop = '40px';
  description.style.textAlign = 'center';
  description.style.color = 'white';

  const dropZone = document.createElement('div');
  dropZone.innerText = 'Drop media here';
  dropZone.style.width = '90%';
  dropZone.style.height = '200px';
  dropZone.style.border = '2px dashed #999';
  dropZone.style.borderRadius = '8px';
  dropZone.style.display = 'flex';
  dropZone.style.alignItems = 'center';
  dropZone.style.justifyContent = 'center';
  dropZone.style.fontSize = '14px';
  dropZone.style.color = '#ccc';

  overlay.appendChild(description);
  overlay.appendChild(dropZone);
  document.body.appendChild(overlay);

  // Drag & Drop handlers
  ['dragenter', 'dragover', 'dragleave', 'drop'].forEach(ev => {
    dropZone.addEventListener(ev, (e) => {
      e.preventDefault();
      e.stopPropagation();
    });
  });
  dropZone.addEventListener('drop', (e) => {
    e.preventDefault();
    e.stopPropagation();
    const files = e.dataTransfer.files;
    if (files.length === 0) return;
    const file = files[0];
    const reader = new FileReader();
    if (file.type.startsWith('image/')) {
      reader.onload = e => {
        document.body.style.backgroundImage = `url('${e.target.result}')`;
        document.body.style.backgroundSize = 'cover';
        document.body.style.backgroundPosition = 'center';
        alert('Background set to the dropped image!');
      };
      reader.readAsDataURL(file);
    } else if (file.type.startsWith('video/')) {
      reader.onload = e => {
        const existingVideo = document.getElementById('custom-video-bg');
        if (existingVideo) existingVideo.remove();
        const video = document.createElement('video');
        video.id = 'custom-video-bg';
        video.src = e.target.result;
        video.style.position = 'fixed';
        video.style.top = '0';
        video.style.left = '0';
        video.style.width = '100%';
        video.style.height = '100%';
        video.style.objectFit = 'cover';
        video.style.zIndex = '-1';
        video.autoplay = true;
        video.loop = true;
        video.muted = true;
        document.body.appendChild(video);
        alert('Background set to the dropped video!');
      };
      reader.readAsDataURL(file);
    } else {
      alert('Please drop an image or a video file.');
    }
  });

  // Make overlay draggable
  let isDragging = false;
  let offsetX = 0;
  let offsetY = 0;
  header.style.cursor = 'move';
  header.addEventListener('mousedown', (e) => {
    isDragging = true;
    offsetX = overlay.offsetLeft - e.clientX;
    offsetY = overlay.offsetTop - e.clientY;
  });
  document.addEventListener('mouseup', () => { isDragging = false; });
  document.addEventListener('mousemove', (e) => {
    if (isDragging) {
      overlay.style.left = e.clientX + offsetX + 'px';
      overlay.style.top = e.clientY + offsetY + 'px';
    }
  });

  // "YaKkScripts" button with green background
  const yakksBtn = document.createElement('button');
  yakksBtn.innerText = 'YaKkScripts';
  yakksBtn.style.position = 'absolute';
  yakksBtn.style.bottom = '10px';
  yakksBtn.style.left = '10px';
  yakksBtn.style.padding = '10px 20px';
  yakksBtn.style.border = 'none';
  yakksBtn.style.borderRadius = '5px';
  yakksBtn.style.backgroundColor = 'green'; // green
  yakksBtn.style.color = 'white';
  yakksBtn.style.cursor = 'pointer';
  yakksBtn.style.fontSize = '14px';
  yakksBtn.style.transition = 'transform 0.3s ease';
  yakksBtn.style.zIndex = '10000';

  yakksBtn.addEventListener('mouseover', () => {
    yakksBtn.style.transform = 'scale(1.2)';
    sound.currentTime = 0;
    sound.play();
  });
  yakksBtn.addEventListener('mouseout', () => {
    yakksBtn.style.transform = 'scale(1)';
  });
  yakksBtn.addEventListener('click', () => {
    window.open('https://sites.google.com/communityschool.us/yakkscripts/home', '_blank');
  });
  overlay.appendChild(yakksBtn);

  // "Drag" label
  const dragLabel = document.createElement('div');
  dragLabel.innerText = 'Drag';
  dragLabel.style.position = 'absolute';
  dragLabel.style.bottom = '10px';
  dragLabel.style.right = '10px';
  dragLabel.style.fontSize = '12px';
  dragLabel.style.color = '#555';
  dragLabel.style.fontStyle = 'italic';
  overlay.appendChild(dragLabel);

  // Toggle button at top-left with smooth slide
  const toggleBtn = document.createElement('button');
  toggleBtn.innerText = 'Open';
  toggleBtn.style.position = 'fixed';
  toggleBtn.style.top = '10px';
  toggleBtn.style.left = '10px';
  toggleBtn.style.padding = '8px 12px';
  toggleBtn.style.border = 'none';
  toggleBtn.style.borderRadius = '5px';
  toggleBtn.style.backgroundColor = '#555';
  toggleBtn.style.color = 'white';
  toggleBtn.style.cursor = 'pointer';
  toggleBtn.style.zIndex = '9999';

  function slideDown() {
    overlay.style.top = '50px';
  }
  function slideUp() {
    overlay.style.top = '-400px';
  }

  toggleBtn.onclick = () => {
    if (overlay.style.top === '-400px') {
      overlay.style.display = 'flex'; // ensure visible
      setTimeout(() => {
        slideDown();
      }, 10);
      toggleBtn.innerText = 'Close';
    } else {
      slideUp();
      toggleBtn.innerText = 'Open';
      // hide after animation
      setTimeout(() => {
        overlay.style.display = 'none';
      }, 500);
    }
  };

  // Keyboard shortcut
  document.addEventListener('keydown', (e) => {
    const isMac = navigator.platform.toUpperCase().indexOf('MAC') >= 0;
    const isCtrlE = (e.ctrlKey && e.key === 'e') || (isMac && e.metaKey && e.key === 'e');
    if (isCtrlE) {
      e.preventDefault();
      toggleBtn.click();
    }
  });

  document.body.appendChild(toggleBtn);
})();


