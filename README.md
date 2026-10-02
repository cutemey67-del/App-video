# App-video
Drama 
<!DOCTYPE html>
<html lang="km">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>កម្មវិធីផលិតរឿង</title>
    <!-- Tailwind CSS សម្រាប់ធ្វើឱ្យទម្រង់ស្អាតនិងងាយស្រួលមើលលើទូរស័ព្ទ -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Kantumruy+Pro:wght@300;400;600;700&display=swap');
        body { font-family: 'Kantumruy Pro', sans-serif; }
    </style>
</head>
<body class="bg-gray-100 min-h-screen pb-20">
    <!-- Header -->
    <header class="bg-indigo-600 text-white p-4 shadow-md sticky top-0 z-10 flex justify-between items-center">
        <h1 class="text-lg font-bold">🎬 កម្មវិធីផលិតរឿង</h1>
        <button onclick="openModal()" class="bg-indigo-700 hover:bg-indigo-800 text-white px-3 py-1.5 rounded-lg text-sm font-semibold shadow">+ បន្ថែមរឿង</button>
    </header>

    <!-- Main Content -->
    <main class="p-4 max-w-md mx-auto">
        <div id="story-list" class="space-y-3">
            <!-- ទិន្នន័យរឿងនឹងត្រូវបង្ហាញទីនេះ -->
        </div>
        <div id="empty-state" class="text-center text-gray-500 mt-10 hidden">
            មិនទាន់មានគម្រោងរឿងនៅឡើយទេ។ សូមចុចប៊ូតុង "+ បន្ថែមរឿង" ដើម្បីបង្កើតថ្មី!
        </div>
    </main>

    <!-- Modal បន្ថែមរឿងថ្មី -->
    <div id="storyModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4 hidden z-20">
        <div class="bg-white rounded-xl p-6 w-full max-w-sm shadow-xl">
            <h2 class="text-lg font-bold mb-4 text-gray-800">បង្កើតគម្រោងរឿងថ្មី</h2>
            <form id="storyForm" onsubmit="saveStory(event)">
                <div class="mb-3">
                    <label class="block text-sm font-medium text-gray-700 mb-1">ចំណងជើងរឿង</label>
                    <input type="text" id="title" required class="w-full border border-gray-300 rounded-lg p-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500" placeholder="ឧ. រឿង អាថ៌កំបាំងព្រៃជ្រៅ">
                </div>
                <div class="mb-3">
                    <label class="block text-sm font-medium text-gray-700 mb-1">ប្រភេទរឿង</label>
                    <input type="text" id="genre" required class="w-full border border-gray-300 rounded-lg p-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500" placeholder="ឧ. រន្ធត់ / ស្នេហា / អាក់សិន">
                </div>
                <div class="mb-4">
                    <label class="block text-sm font-medium text-gray-700 mb-1">ស្ថានភាព</label>
                    <select id="status" class="w-full border border-gray-300 rounded-lg p-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                        <option value="កំពុងសរសេរ">កំពុងសរសេរ</option>
                        <option value="រៀបចំផែនការ">រៀបចំផែនការ</option>
                        <option value="កំពុងថត">កំពុងថត</option>
                        <option value="បានរួចរាល់">បានរួចរាល់</option>
                    </select>
                </div>
                <div class="flex space-x-2">
                    <button type="button" onclick="closeModal()" class="w-1/2 bg-gray-200 text-gray-800 py-2 rounded-lg text-sm font-semibold">បោះបង់</button>
                    <button type="submit" class="w-1/2 bg-indigo-600 text-white py-2 rounded-lg text-sm font-semibold">រក្សាទុក</button>
                </div>
            </form>
        </div>
    </div>

    <!-- JavaScript សម្រាប់ដំណើរការ App -->
    <script>
        // ទាញយកទិន្នន័យពី LocalStorage របស់ទូរស័ព្ទ
        let stories = JSON.parse(localStorage.getItem('stories')) || [
            { id: 1, title: 'រឿង អាថ៌កំបាំងព្រៃជ្រៅ', genre: 'រន្ធត់ / Mystery', status: 'កំពុងសរសេរ' },
            { id: 2, title: 'រឿង ក្តីស្រមៃក្រោមកាំរស្មីព្រះអាទិត្យ', genre: 'ស្នេហា / Romance', status: 'កំពុងថត' }
        ];

        function renderStories() {
            const listEl = document.getElementById('story-list');
            const emptyEl = document.getElementById('empty-state');
            listEl.innerHTML = '';

            if (stories.length === 0) {
                emptyEl.classList.remove('hidden');
                return;
            } else {
                emptyEl.classList.add('hidden');
            }

            stories.forEach(story => {
                let statusBg = 'bg-yellow-100 text-yellow-800';
                if (story.status === 'កំពុងថត') statusBg = 'bg-blue-100 text-blue-800';
                if (story.status === 'បានរួចរាល់') statusBg = 'bg-green-100 text-green-800';

                const card = document.createElement('div');
                card.className = 'bg-white p-4 rounded-xl shadow-sm border border-gray-200 flex justify-between items-start';
                card.innerHTML = `
                    <div>
                        <h3 class="font-bold text-gray-800 text-base">${story.title}</h3>
                        <p class="text-xs text-gray-500 mt-1">ប្រភេទ: ${story.genre}</p>
                        <span class="inline-block mt-2 px-2.5 py-0.5 rounded-full text-xs font-semibold ${statusBg}">${story.status}</span>
                    </div>
                    <button onclick="deleteStory(${story.id})" class="text-red-400 hover:text-red-600 p-1 text-sm">🗑️</button>
                `;
                listEl.appendChild(card);
            });
        }

        function openModal() {
            document.getElementById('storyModal').classList.remove('hidden');
        }

        function closeModal() {
            document.getElementById('storyModal').classList.add('hidden');
            document.getElementById('storyForm').reset();
        }

        function saveStory(e) {
            e.preventDefault();
            const title = document.getElementById('title').value;
            const genre = document.getElementById('genre').value;
            const status = document.getElementById('status').value;

            const newStory = {
                id: Date.now(),
                title,
                genre,
                status
            };

            stories.push(newStory);
            localStorage.setItem('stories', JSON.stringify(stories));
            renderStories();
            closeModal();
        }

        function deleteStory(id) {
            if (confirm('តើអ្នកពិតជាចង់លុបគម្រោងនេះមែនទេ?')) {
                stories = stories.filter(s => s.id !== id);
                localStorage.setItem('stories', JSON.stringify(stories));
                renderStories();
            }
        }

        renderStories();
    </script>
</body>
</html>
