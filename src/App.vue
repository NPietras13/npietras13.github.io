<template>
  <div class="tutorial-container">
    <h1>Tutorial: Konfiguracja Laravel+Vue+InertiaJS na Windows</h1>

    <!-- Nawigacja po Krokach -->
    <div class="steps-nav">
      <button
          v-for="(step, index) in steps"
          :key="index"
          @click="currentStep = index"
          :class="{ active: currentStep === index }"
      >
        {{ step.title }}
      </button>
    </div>

    <!-- Treść Kroku -->
    <div class="step-content">
      <h2>{{ steps[currentStep].title }}</h2>
      <p>{{ steps[currentStep].description }}</p>

      <!-- Blok Kod -->
      <div class="pre code" v-if="steps[currentStep].code">
        <code>{{ steps[currentStep].code }}</code>
      </div>

      <div class="pre response" v-if="steps[currentStep].response">
        <code>{{ steps[currentStep].response }}</code>
      </div>

      <!-- Dodatkowa Uwagi -->
      <p v-if="steps[currentStep].note" class="note">{{ steps[currentStep].note }}</p>

      <!-- Przycisk Poprzedni/Następny -->
      <div class="step-actions">
        <button @click="previousStep" :disabled="currentStep === 0">Poprzedni</button>
        <button @click="nextStep" :disabled="currentStep === steps.length - 1">
          Następny
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentStep: 0,
      steps: [
        {
          title: "Krok 1: Instalacja XAMPP",
          description: "Aby uruchomić Laravel na Windows, zainstaluj XAMPP, który dostarcza PHP, Apache i MySQL. Pobierz go z oficjalnej strony:",
          note: "Odwiedź https://www.apachefriends.org/index.html, aby pobrać najnowszą wersję XAMPP.",
        },
        {
          title: "Krok 2: Instalacja Composera",
          description: "Composer to menedżer zależności dla PHP. Pobierz i zainstaluj go globalnie na swoim systemie.",
          code: "composer -v",
          response: "Composer version 2.5.7 2023-05-24 15:00:39\n",
        },
        {
          title: "Krok 3: Instalacja Laravel",
          description: "Użyj Composera, aby zainstalować Laravel globalnie.",
          code: "composer global require laravel/installer",
          response: "Using version ^5.11 for laravel/installer",
          note: "Upewnij się, że 'Composer' jest dodany do zmiennej systemowej PATH.",
        },
        {
          title: "Krok 4: Utwórz Projekt Laravel",
          description: "Uruchom poniższą komendę, aby utworzyć nowy projekt Laravel.",
          code: "laravel new my-laravel-app",
        },
        {
          title: "Krok 5: Uruchom Serwer Deweloperski",
          description: "Przejdź do katalogu projektu i uruchom serwer.",
          code: "cd my-laravel-app\nnpm install\nnpm run dev\nphp artisan serve",
          note: "Uruchom komendy npm run dev(frontend) oraz php artisan serve(backend).",
        },
        {
          title: "Krok 6: Otwórz Projekt w Przeglądarce",
          description: "Otwórz poniższy adres w przeglądarce, aby zobaczyć stronę powitalną Laravel:",
          code: "http://127.0.0.1:8000",
        },
        {
          title: "Krok 7: Utwórz Model i Migrację",
          description: "Stwórz prosty model `Post` wraz z migracją dla bazy danych.",
          code: "php artisan make:model Post -m",
          response: "Model created successfully.\nCreated Migration: 2024_01_01_000000_create_posts_table",
        },
        {
          title: "Krok 8: Zdefiniuj Schemat Bazy Danych",
          description: "Edytuj plik migracji, aby dodać kolumny `title` oraz `content`.",
          code: `
public function up()
{
    Schema::create('posts', function (Blueprint $table) {
        $table->id();
        $table->string('title');
        $table->text('content');
        $table->timestamps();
    });
}
          `,
        },
        {
          title: "Krok 9: Uruchom Migrację",
          description: "Uruchom migrację bazy danych, aby stworzyć tabelę `posts`.",
          code: "php artisan migrate",
          response: "create_posts_table ...................................... 79.21ms DONE",
        },
        {
          title: "Krok 10: Utwórz Kontroler",
          description: "Stwórz kontroler dla modelu Post.",
          code: "php artisan make:controller PostController",
          response: "Controller created successfully.",
        },
        {
          title: "Krok 11: Zdefiniuj Routy",
          description: "Dodaj trasy dla kontrolera Post w `routes/web.php`.",
          code: `
use App\Http\Controllers\PostController;

Route::get('/', [PostController::class, 'index'])->name('posts.index');
Route::get('/{post}', [PostController::class, 'show'])->name('posts.show');
Route::get('/create', [PostController::class, 'create'])->name('posts.create');
Route::post('/', [PostController::class, 'store'])->name('posts.store');
          `,
        },
        {
          title: "Krok 12: Utwórz Widoki Inertia",
          description: "Stwórz komponenty Vue dla tras zdefiniowanych wcześniej.",
          note: "Możesz użyć funkcji route('route.name') zamiast wpisywać adres URL statycznie.",
          code: '' +
              '<!-- Index.vue - Lista Postów -->\n' +
              '<template>\n' +
              '    <div>\n' +
              '        <h1>All Posts</h1>\n' +
              '        <ul>\n' +
              '            <li v-for="post in posts" :key="post.id">\n' +
              '                <Link :href="`/${post.id}`">{{ post.title }}</Link>\n' +
              '            </li>\n' +
              '        </ul>\n' +
              '    </div>\n' +
              '</template>\n\n' +
              '<script setup>\n' +
              'import { Link } from "@inertiajs/vue3";\n' +
              'const props = defineProps([\'posts\'])\n' +
              '<' + '/script>\n\n\n' +
              '<!-- Show.vue - Wyświetlanie postu -->\n' +
              '<template>\n' +
              '    <div>\n' +
              '        <h1>{{ post.title }}</h1>\n' +
              '        <p>{{ post.content }}</p>\n' +
              '    </div>\n' +
              '</template>\n\n' +
              '<script setup>\n' +
              'const props = defineProps([\'post\']);\n' +
              '<'+'/script>\n\n\n' +
              '<!-- Create.vue - Formularz do tworzenia postu -->\n' +
              '<template>\n' +
              '    <div>\n' +
              '        <form @submit.prevent="submit">\n' +
              '            <label>Title:</label>\n' +
              '            <input v-model="form.title" required />\n' +
              '\n' +
              '            <label>Content:</label>\n' +
              '            <textarea v-model="form.content" required></textarea>\n' +
              '\n' +
              '            <button type="submit">Save</button>\n' +
              '        </form>\n' +
              '    </div>\n' +
              '</template>\n\n' +
              '<script setup>\n' +
              'import { useForm } from "@inertiajs/vue3";\n' +
              'const form = useForm({ title: "", content: "" });\n\n' +
              'function submit() {\n' +
              '    form.post("/");\n' +
              '}\n' +
              '<'+'/script>',
    },
  ],
  }
    ;
  },
  methods: {
    nextStep() {
      if (this.currentStep < this.steps.length - 1) this.currentStep++;
    },
    previousStep() {
      if (this.currentStep > 0) this.currentStep--;
    },
  },
};
</script>

<style scoped>
.tutorial-container {
  font-family: Arial, sans-serif;
  max-width: 800px;
  margin: auto;
  padding: 20px;
  background-color: #f9f9f9;
  border: 1px solid #ddd;
  border-radius: 8px;
  color: #272822;
}

h1 {
  text-align: center;
  color: #333;
}

.steps-nav {
  flex-wrap: wrap;
  display: flex;
  justify-content: center;
  gap: 10px;
  margin: 20px 0;
}

.steps-nav button {
  padding: 10px 20px;
  border: 1px solid #ddd;
  background: #f4f4f4;
  cursor: pointer;
  transition: all 0.3s ease;
}

.steps-nav button.active {
  background-color: #42b983;
  color: #fff;
  border-color: #42b983;
}

.step-content {
  background: #fff;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
}

.pre {
  background: #272822;
  color: #f8f8f2;
  padding: 15px;
  overflow: auto;
  border-radius: 5px;
  white-space: pre-wrap;
  height: fit-content;
}

.code {
  border-left: 5px solid blue;
}

.response {
  margin-top: 10px;
  border-left: 5px solid lightgreen;
}

.note {
  background-color: #fffae6;
  color: #856404;
  padding: 10px;
  border-left: 5px solid #ffecb3;
  margin-top: 10px;
}

.step-actions {
  display: flex;
  justify-content: space-between;
  margin-top: 20px;
}

.step-actions button {
  padding: 10px 20px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.step-actions button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}
</style>
