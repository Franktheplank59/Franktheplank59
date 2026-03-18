(function() {
    // 1. Cleanup old elements
    if (window.ovl) window.ovl.remove();
    if (window.rem) window.rem.remove();
    if (window.activeInt) clearInterval(window.activeInt);

    // 2. Create Overlay
    window.ovl = document.createElement('div');
    var s = window.ovl.style;
    s.position = 'fixed'; s.top = '0'; s.left = '0'; s.width = '100vw'; s.height = '100vh';
    s.backgroundColor = 'rgba(0,0,0,0.85)'; s.zIndex = '2147483645'; s.pointerEvents = 'none';
    s.display = 'flex'; s.justifyContent = 'center'; s.alignItems = 'center';
    s.color = 'red'; s.fontFamily = 'monospace'; s.fontSize = '40px';
    document.documentElement.appendChild(window.ovl);

    // 3. Create Remote
    window.rem = document.createElement('div');
    var r = window.rem.style;
    r.position = 'fixed'; r.top = '20px'; r.left = '20px'; r.width = '160px';
    r.background = '#000'; r.border = '1px solid #0f0'; r.padding = '10px';
    r.zIndex = '2147483647'; r.color = '#0f0'; r.textAlign = 'center';
    r.fontFamily = 'monospace'; r.cursor = 'move';
    window.rem.textContent = 'STABLE_v11';

    // 4. Function to clear current effects
    function resetEffects() {
        if (window.activeInt) clearInterval(window.activeInt);
        document.body.style.filter = 'none';
        window.ovl.style.backgroundColor = 'rgba(0,0,0,0.85)';
        window.ovl.textContent = '';
        var input = document.querySelector('textarea') || document.querySelector('input');
        if (input) input.value = '';
    }

    // 5. Button Builder
    function addBtn(label, color, action) {
        var b = document.createElement('button');
        b.textContent = label;
        var bs = b.style;
        bs.width = '100%'; bs.marginTop = '6px'; bs.background = '#111';
        bs.color = color; bs.border = '1px solid ' + color;
        bs.fontSize = '10px'; bs.cursor = 'pointer'; bs.fontFamily = 'monospace';
        
        b.onclick = function() {
            resetEffects();
            action();
        };
        window.rem.appendChild(b);
    }

    // --- EXCLUSIVE MODULES ---

    addBtn('GHOST TYPE', '#fff', function() {
        var msg = "RUNNING...";
        var i = 0;
        window.activeInt = setInterval(function() {
            window.ovl.textContent += msg.charAt(i);
            i++;
            if (i >= msg.length) i = 0; // Loops
        }, 200);
    });

    addBtn('THERMAL', '#ff0', function() {
        document.body.style.filter = 'invert(1) hue-rotate(180deg)';
        window.ovl.style.backgroundColor = 'transparent';
    });

    addBtn('RED ALERT', '#f00', function() {
        var state = true;
        window.activeInt = setInterval(function() {
            window.ovl.style.backgroundColor = state ? 'rgba(255,0,0,0.5)' : 'rgba(0,0,0,0.9)';
            state = !state;
        }, 500);
    });

    addBtn('HIJACK', '#0f0', function() {
        var input = document.querySelector('textarea') || document.querySelector('input');
        if (input) {
            var txt = "who is there? ";
            var j = 0;
            window.activeInt = setInterval(function() {
                input.value += txt.charAt(j);
                j++;
                if (j >= txt.length) j = 0;
            }, 150);
        }
    });

    addBtn('REBOOT', '#888', function() { location.reload(); });

    document.documentElement.appendChild(window.rem);

    // 6. Movement Logic
    var drag = false, ox, oy;
    window.rem.onmousedown = function(e) { drag = true; ox = e.clientX - window.rem.offsetLeft; oy = e.clientY - window.rem.offsetTop; };
    window.onmousemove = function(e) { if(drag) { window.rem.style.left = e.clientX-ox+'px'; window.rem.style.top = e.clientY-oy+'px'; } };
    window.onmouseup = function() { drag = false; };

})();
