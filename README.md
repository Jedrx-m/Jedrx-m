<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jedrick's Coding Vibes</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <link href="https://fonts.googleapis.com/css2?family=Fira+Code:wght@400;600;700&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; }
        .code-font { font-family: 'Fira Code', monospace; }
        @keyframes pulse-slow {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.4; }
        }
        .animate-pulse-slow { animation: pulse-slow 2s cubic-bezier(0.4, 0, 0.6, 1) infinite; }
    </style>
</head>
<body class="bg-slate-950 text-slate-100 min-h-screen flex flex-col items-center justify-center p-4">

    <div class="w-full max-w-3xl space-y-6">
        <!-- Header badges -->
        <div class="flex flex-wrap items-center justify-center gap-3">
            <span class="inline-flex items-center gap-2 px-4 py-2 rounded-xl bg-purple-950/60 border border-purple-800/50 text-purple-300 font-medium text-sm shadow-lg shadow-purple-950/30 backdrop-blur-md">
                <span class="w-2.5 h-2.5 rounded-full bg-purple-400 animate-ping"></span>
                Status: Vibing & Coding
            </span>
            <span class="inline-flex items-center gap-2 px-4 py-2 rounded-xl bg-amber-950/60 border border-amber-800/50 text-amber-300 font-medium text-sm shadow-lg shadow-amber-950/30 backdrop-blur-md">
                <span class="w-2.5 h-2.5 rounded-full bg-amber-400"></span>
                Current Mood: Debugging
            </span>
        </div>

        <!-- Main Code Window / Live Banner -->
        <div class="rounded-2xl bg-slate-900/90 border border-slate-800 shadow-2xl overflow-hidden backdrop-blur-xl">
            <!-- Window Titlebar -->
            <div class="flex items-center justify-between px-4 py-3 bg-slate-950/50 border-b border-slate-800/80">
                <div class="flex items-center space-x-2">
                    <div class="w-3 h-3 rounded-full bg-rose-500/80"></div>
                    <div class="w-3 h-3 rounded-full bg-amber-500/80"></div>
                    <div class="w-3 h-3 rounded-full bg-emerald-500/80"></div>
                    <span class="ml-2 text-xs font-mono text-slate-400">jedrick-workspace.js</span>
                </div>
                <div class="flex items-center space-x-2">
                    <button id="speed-btn" onclick="toggleSpeed()" class="text-xs px-2.5 py-1 rounded-lg bg-slate-800 hover:bg-slate-700 text-slate-300 font-mono transition">
                        Speed: 1x
                    </button>
                    <button onclick="resetAnimation()" class="text-xs px-2.5 py-1 rounded-lg bg-slate-800 hover:bg-slate-700 text-slate-300 font-mono transition">
                        Restart
                    </button>
                </div>
            </div>

            <!-- Code Content Area -->
            <div class="p-6 md:p-8 code-font text-sm md:text-base leading-relaxed overflow-x-auto min-h-[160px] flex items-center">
                <div id="code-output" class="text-slate-300"></div>
                <span class="inline-block w-2.5 h-5 bg-pink-500 ml-1 animate-pulse"></span>
            </div>

            <!-- Footer / Vibe Status Bar -->
            <div class="px-6 py-3 bg-slate-950/80 border-t border-slate-800/80 flex items-center justify-between text-xs text-slate-400 font-mono">
                <div class="flex items-center space-x-3">
                    <span class="text-pink-400">UTF-8</span>
                    <span>JavaScript</span>
                </div>
                <div id="vibe-ticker" class="text-purple-400 animate-pulse-slow">
                    ☕ coffee.refill() -> running...
                </div>
            </div>
        </div>

        <!-- Customizer Panel -->
        <div class="bg-slate-900/50 border border-slate-800/60 rounded-2xl p-5 backdrop-blur-md space-y-4">
            <h3 class="text-sm font-semibold text-slate-300 uppercase tracking-wider">Customize Your Live Banner Code</h3>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <div>
                    <label class="block text-xs font-medium text-slate-400 mb-1">Developer Name</label>
                    <input type="text" id="input-name" value="Jedrick" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2 text-slate-200 text-sm focus:outline-none focus:border-pink-500 transition" oninput="updateSnippet()">
                </div>
                <div>
                    <label class="block text-xs font-medium text-slate-400 mb-1">Current Status</label>
                    <input type="text" id="input-status" value="Coding & Vibing" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-3 py-2 text-slate-200 text-sm focus:outline-none focus:border-pink-500 transition" oninput="updateSnippet()">
                </div>
            </div>
        </div>
    </div>

    <script>
        let speedMultiplier = 1;
        let charIndex = 0;
        let isDeleting = false;
        let textTimer = null;

        function getSnippets() {
            const name = document.getElementById('input-name').value || 'Jedrick';
            const status = document.getElementById('input-status').value || 'Coding';
            return [
                `const developer = "${name}";\nlet currentStatus = "${status}";\n\nwhile (true) {\n    code();\n    drinkCoffee();\n    fixBugs();\n}`,
                `async function unlockSuccess() {\n    const coder = "${name}";\n    await coder.drinkCoffee();\n    while(isDebugging) {\n        console.log("Making magic happen ✨");\n    }\n}`,
                `const ${name.toLowerCase()} = {\n    role: "Full-Stack Developer",\n    status: "${status}",\n    stack: ["React", "Node", "Python"],\n    coffeeLevel: "Infinity ☕"\n};`
            ];
        }

        let currentSnippetIdx = 0;

        function formatCodeToHTML(text) {
            // Simple custom syntax highlighting renderer for clean output
            return text
                .replace(/(const|let|async|function|while)/g, '<span class="text-purple-400 font-semibold">$1</span>')
                .replace(/(true|false)/g, '<span class="text-amber-400">$1</span>')
                .replace(/(".*?")/g, '<span class="text-emerald-300">$1</span>')
                .replace(/(\b[a-zA-Z_][a-zA-Z0-9_]*\()/g, '<span class="text-blue-400">$1</span>')
                .replace(/\n/g, '<br>');
        }

        function typeWriter() {
            const snippets = getSnippets();
            const targetText = snippets[currentSnippetIdx];
            const outputEl = document.getElementById('code-output');

            if (!isDeleting) {
                charIndex++;
                if (charIndex > targetText.length) {
                    isDeleting = true;
                    textTimer = setTimeout(typeWriter, 2500 / speedMultiplier); // Pause at end
                    return;
                }
            } else {
                charIndex--;
                if (charIndex < 0) {
                    isDeleting = false;
                    currentSnippetIdx = (currentSnippetIdx + 1) % snippets.length;
                    textTimer = setTimeout(typeWriter, 500 / speedMultiplier);
                    return;
                }
            }

            const currentSubstr = targetText.substring(0, charIndex);
            outputEl.innerHTML = formatCodeToHTML(currentSubstr);

            const typingSpeed = isDeleting ? 30 : (Math.random() * 40 + 40);
            textTimer = setTimeout(typeWriter, typingSpeed / speedMultiplier);
        }

        function toggleSpeed() {
            if (speedMultiplier === 1) speedMultiplier = 2;
            else if (speedMultiplier === 2) speedMultiplier = 3;
            else speedMultiplier = 1;
            document.getElementById('speed-btn').innerText = `Speed: ${speedMultiplier}x`;
        }

        function resetAnimation() {
            clearTimeout(textTimer);
            charIndex = 0;
            isDeleting = false;
            typeWriter();
        }

        function updateSnippet() {
            clearTimeout(textTimer);
            charIndex = 0;
            isDeleting = false;
            typeWriter();
        }

        // Start animation on load
        window.onload = function() {
            typeWriter();
        };
    </script>
</body>
</html>
