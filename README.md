<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GitHub Profile README Coding Vibes Banner Generator</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <link href="https://fonts.googleapis.com/css2?family=Fira+Code:wght@400;600&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; }
        .code-font { font-family: 'Fira Code', monospace; }
    </style>
</head>
<body class="bg-slate-950 text-slate-100 min-h-screen flex flex-col items-center justify-center p-4">

    <div class="w-full max-w-4xl space-y-6">
        <!-- Header -->
        <div class="text-center space-y-2">
            <h1 class="text-2xl md:text-3xl font-bold bg-gradient-to-r from-pink-400 via-purple-400 to-indigo-400 bg-clip-text text-transparent">
                GitHub Profile README Vibes Generator
            </h1>
            <p class="text-slate-400 text-sm">Create a lightning-fast, custom animated SVG typing banner for your GitHub profile.</p>
        </div>

        <!-- Live Preview Box -->
        <div class="rounded-2xl bg-slate-900 border border-slate-800 shadow-2xl p-6 flex flex-col items-center justify-center space-y-6">
            <div class="text-xs uppercase tracking-wider font-mono text-slate-500">Live SVG Preview</div>
            
            <!-- Preview Render Container -->
            <div id="svg-preview-container" class="w-full overflow-x-auto flex justify-center p-2 bg-slate-950 rounded-xl border border-slate-800/80">
                <!-- SVG injected via JS -->
            </div>

            <!-- Badges preview -->
            <div class="flex flex-wrap items-center justify-center gap-3 pt-2">
                <img id="badge-status-preview" src="https://img.shields.io/badge/Status-Vibing_%26_Coding-blueviolet?style=for-the-badge&logo=codeforces&logoColor=white" alt="Status" />
                <img id="badge-mood-preview" src="https://img.shields.io/badge/Current_Mood-Debugging-orange?style=for-the-badge&logo=coffeescript&logoColor=white" alt="Mood" />
            </div>
        </div>

        <!-- Customizer Controls -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4 bg-slate-900/60 border border-slate-800/80 rounded-2xl p-6 backdrop-blur-md">
            <div class="space-y-4">
                <h3 class="text-sm font-semibold text-slate-300 uppercase tracking-wider">Configure Code Content</h3>
                <div>
                    <label class="block text-xs font-medium text-slate-400 mb-1">Developer Name</label>
                    <input type="text" id="cfg-name" value="Jedrick" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2 text-slate-200 text-sm focus:outline-none focus:border-pink-500 transition" oninput="generateSVG()">
                </div>
                <div>
                    <label class="block text-xs font-medium text-slate-400 mb-1">Custom Code Line</label>
                    <input type="text" id="cfg-code" value="while(true) { vibe(); }" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2 text-slate-200 text-sm font-mono focus:outline-none focus:border-pink-500 transition" oninput="generateSVG()">
                </div>
            </div>

            <div class="space-y-4">
                <h3 class="text-sm font-semibold text-slate-300 uppercase tracking-wider">Animation Speed & Theme</h3>
                <div>
                    <label class="block text-xs font-medium text-slate-400 mb-1">Typing Speed (Duration)</label>
                    <select id="cfg-speed" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2 text-slate-200 text-sm focus:outline-none focus:border-pink-500 transition" onchange="generateSVG()">
                        <option value="3s">Fast (3 seconds)</option>
                        <option value="5s" selected>Normal / Balanced (5 seconds)</option>
                        <option value="8s">Slow & Relaxed (8 seconds)</option>
                    </select>
                </div>
                <div>
                    <label class="block text-xs font-medium text-slate-400 mb-1">Accent Color</label>
                    <select id="cfg-color" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2 text-slate-200 text-sm focus:outline-none focus:border-pink-500 transition" onchange="generateSVG()">
                        <option value="#FF79C6">Neon Pink (Dracula)</option>
                        <option value="#50FA7B">Neon Green</option>
                        <option value="#8BE9FD">Cyan Blue</option>
                        <option value="#FFB86C">Warm Orange</option>
                    </select>
                </div>
            </div>
        </div>

        <!-- Markdown Output Box -->
        <div class="bg-slate-900/80 border border-slate-800 rounded-2xl p-6 space-y-3">
            <div class="flex items-center justify-between">
                <span class="text-sm font-semibold text-slate-300 uppercase tracking-wider">Copy Markdown for your README.md</span>
                <button onclick="copyMarkdown()" class="px-4 py-1.5 rounded-xl bg-pink-600 hover:bg-pink-500 text-white text-xs font-medium transition shadow-lg shadow-pink-600/30">
                    Copy Markdown
                </button>
            </div>
            <textarea id="markdown-output" readonly rows="4" class="w-full bg-slate-950 border border-slate-800 rounded-xl p-3 text-xs font-mono text-pink-300 focus:outline-none resize-none"></textarea>
        </div>
    </div>

    <script>
        function generateSVG() {
            const name = document.getElementById('cfg-name').value || 'Jedrick';
            const codeLine = document.getElementById('cfg-code').value || 'while(true) { vibe(); }';
            const speed = document.getElementById('cfg-speed').value;
            const color = document.getElementById('cfg-color').value;

            // Constructing clean Heroku typing SVG URL which is lightning fast and standard for GitHub profiles
            const line1 = encodeURIComponent(`const name = "${name}";`);
            const line2 = encodeURIComponent(`let status = "Coding";`);
            const line3 = encodeURIComponent(codeLine);
            
            const typingSvgUrl = `https://readme-typing-svg.herokuapp.com?font=Fira+Code&weight=700&size=20&pause=1200&color=${color.replace('#','')}&center=true&vCenter=true&width=560&height=70&lines=${line1};${line2};${line3}`;

            const markdown = `<p align="center">
  <img src="${typingSvgUrl}" alt="Typing SVG" />
</p>
<br />
<p align="center">
  <img src="https://img.shields.io/badge/Status-Vibing_%26_Coding-blueviolet?style=for-the-badge&logo=codeforces&logoColor=white" />
  <img src="https://img.shields.io/badge/Current_Mood-Debugging-orange?style=for-the-badge&logo=coffeescript&logoColor=white" />
</p>`;

            document.getElementById('svg-preview-container').innerHTML = `<img src="${typingSvgUrl}" alt="Typing Preview" />`;
            document.getElementById('markdown-output').value = markdown;
        }

        function copyMarkdown() {
            const textarea = document.getElementById('markdown-output');
            textarea.select();
            document.execCommand('copy');
            alert('Markdown copied to clipboard!');
        }

        window.onload = function() {
            generateSVG();
        };
    </script>
</body>
</html>
