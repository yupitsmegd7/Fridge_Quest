<div align="center">
  <img src="public/chef-mascot.webp" alt="Pip, the Fridge Quest chef companion" width="220" />

  # Fridge Quest 🍳

  **Your fridge is the inventory. Your next meal is the quest.**

  A playful cooking app that turns everyday ingredients into recipe ideas, guided cooking steps, and easy-to-read nutrition estimates.

  ![Next.js](https://img.shields.io/badge/Next.js-16-111827?style=flat-square&logo=nextdotjs)
  ![React](https://img.shields.io/badge/React-19-61DAFB?style=flat-square&logo=react&logoColor=111827)
  ![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?style=flat-square&logo=typescript&logoColor=white)
  ![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-4-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white)

  [Features](#-what-you-can-do) · [Quick start](#-run-it-locally) · [AI setup](#-enable-ai-recipes-optional) · [Deployment](#-deploy-your-own-copy)
</div>

---

## 🧑‍🍳 Meet your kitchen sidekick

Fridge Quest makes the question **“What can I cook with what I already have?”** feel like a small adventure. Pip guides you through choosing a craving, exploring cuisines, and adding ingredients. Pick a recipe, follow its steps, and collect XP when you finish cooking.

**It works without an API key.** The app includes 24 recipes and an ingredient-matching engine. Add your own OpenAI API key to enable live recipe creation; the interface clearly labels which mode supplied the results.

## ✨ What you can do

| Feature | How it works |
| --- | --- |
| Choose a craving | Quick bite, protein power, comfort food, or fresh & light |
| Explore cuisines | Indian, Italian, Asian-inspired, Mexican-inspired, Mediterranean, or “Surprise me” |
| Build your fridge inventory | Enter names separated by commas, semicolons, or newlines; use quick-add buttons for common ingredients |
| Find recipe matches | See recipes ranked by ingredient coverage and, for quick/protein/light choices, the selected preference |
| Check what is missing | Each recipe lists ingredients you still need before cooking |
| Read nutrition estimates | Calories, protein, carbohydrates, fat, and fiber per serving |
| Scale the ingredients | Adjust a recipe from one to six servings |
| Follow cooking steps | Check off instructions and see your cooking progress |
| Earn chef XP | Finish a recipe to collect 50 XP for the current session |
| Cook on any screen | Responsive layouts, keyboard-accessible controls, and reduced-motion support |

### The adventure

1. **Set the mood:** choose your craving and cuisine.
2. **Raid the fridge:** add the ingredients you actually have.
3. **Discover recipes:** check the missing items, open a recipe, and start cooking.

Try **eggs + spinach** with **Surprise me** and the pantry-basics option enabled to find the everyday omelette.

## 🚀 Run it locally

### Requirements

- **Node.js 22.13 or newer**; the repository includes an `.nvmrc` for Node 22.
- **pnpm 11.25.0**, matching the `packageManager` field.
- An OpenAI API key is **optional**.

Download or clone your copy of the repository, then open a terminal in its root folder:

```bash
corepack enable
corepack prepare pnpm@11.25.0 --activate
pnpm install --frozen-lockfile
pnpm dev
```

Open **[localhost:3000](http://localhost:3000)**.

If Corepack is unavailable, install the pinned package manager with `npm install -g pnpm@11.25.0`, then run the last two commands. On Windows, you can use Command Prompt if PowerShell blocks package-manager scripts.

### Commands

| Command | Purpose |
| --- | --- |
| `pnpm dev` | Start the local development server |
| `pnpm typecheck` | Check TypeScript types |
| `pnpm build` | Create a production build using Next.js and webpack |
| `pnpm start` | Serve the production build; run `pnpm build` first |

## 🤖 Enable AI recipes — optional

The built-in collection is available immediately. To generate new recipes from your inputs:

1. Copy `.env.example` to `.env.local`.
2. Add your API key to the new file.
3. Restart the development server.

```dotenv
OPENAI_API_KEY=your_api_key_here
OPENAI_MODEL=gpt-4o-mini
```

On macOS or Linux:

```bash
cp .env.example .env.local
```

On Windows PowerShell:

```powershell
Copy-Item .env.example .env.local
```

| Variable | Required? | Purpose |
| --- | --- | --- |
| `OPENAI_API_KEY` | Only for live AI | Server-side API credential |
| `OPENAI_MODEL` | No | Structured-output-capable model; defaults to `gpt-4o-mini` |

The browser calls `/api/recipes`; only the server communicates with OpenAI. Requests and model responses are validated with Zod. The endpoint uses structured JSON output, request size limits, and a provider timeout. If the provider fails or no key is configured, it returns matches from the built-in collection with an explicit notice.

**Never commit `.env.local` or put an API key in a `NEXT_PUBLIC_` variable.** Environment files are ignored by Git; `.env.example` contains placeholders only. API calls use your own provider account and billing.

## 🌍 Deploy your own copy

**GitHub stores the code. A Next.js-compatible host runs the website and its server API.** This project is a standalone Next.js app and does not require ChatGPT Sites, its hosting credentials, or a Cloudflare account.

### With a Git-connected Next.js host

1. Push this repository to your GitHub account.
2. Import it into a host that supports full Next.js applications, such as Vercel.
3. Select the **Next.js** framework if it is not detected automatically.
4. Use `pnpm install --frozen-lockfile` for installation and `pnpm build` for the build.
5. Optionally set `OPENAI_API_KEY` as a private environment variable and `OPENAI_MODEL` as desired.
6. Deploy. Adding or changing an environment variable generally requires another deployment.

### On a Node.js server

```bash
pnpm install --frozen-lockfile
pnpm build
pnpm start --hostname 0.0.0.0
```

Set the host's `PORT` environment variable when it requires a specific port. The host must allow outbound HTTPS requests to `api.openai.com` for live AI and allow requests long enough for the configured AI timeout.

**GitHub Pages alone is not suitable for this version**, because `/api/recipes` requires a server. Do not switch to a static export if you want the live AI endpoint to keep working.

Before enabling a paid API on a public deployment, add access controls and rate limiting. They are not included in this version.

See the official [Next.js deployment guide](https://nextjs.org/docs/app/getting-started/deploying) for supported deployment options.

## 🧩 How it is built

| Layer | Technology |
| --- | --- |
| Application | Next.js 16 App Router + React 19 |
| Language | TypeScript |
| Styling | Tailwind CSS 4, custom CSS, and shared design tokens |
| Controls | Radix primitives with selected shadcn components |
| Icons | Lucide React |
| Validation | Zod |
| Recipe data | Local TypeScript collection and ingredient reference values |
| Optional AI | OpenAI Responses API with structured outputs |
| Persistence | In-memory session state; no database |

### Key files

| Path | Responsibility |
| --- | --- |
| `app/page.tsx` | Quest flow, fridge input, results, recipe dialog, and XP |
| `app/recipes.ts` | Recipe collection, aliases, nutrient estimates, and matching logic |
| `app/api/recipes/route.ts` | Validated server endpoint, optional AI calls, and fallback behavior |
| `app/globals.css` | Game-inspired design and responsive layouts |
| `app/layout.tsx` | Page metadata and root layout |
| `components/ui/` | Accessible button, checkbox, dialog, radio, and progress primitives |
| `public/chef-mascot.webp` | Original AI-generated illustration of Pip |
| `public/favicon.svg` | App icon |
| `.env.example` | Optional server configuration template |

### Recipe matching

The built-in matcher normalizes common names such as `eggs → egg`, `curd → yogurt`, and `capsicum → pepper`. It filters by cuisine, prioritizes complete ingredient matches, and ranks remaining recipes by ingredient coverage. The quick, protein, and light preferences influence ranking; comfort food uses the base ranking.

Recipes can still appear when you are missing ingredients, but the missing items are shown explicitly. Unknown ingredients are retained in the input and flagged as possibly unsupported by the built-in collection.

## 🥗 Nutrition and current boundaries

- Nutrition is **approximate**, calculated from generic ingredient reference values and the listed recipe amounts, including oil. It is not a live food-database lookup.
- Brands, cooking methods, preparation state, and actual portions affect the result. AI-generated nutrition is also an estimate.
- Scaling servings changes the ingredient amounts. The nutrition panel remains **per serving**.
- Ingredient entry matches names; it does **not** track how much food remains in your fridge.
- Cooked rice is distinct from dry rice. Built-in bean and lentil recipes use cooked or canned portions.
- The app has no allergy filter, medical nutrition planning, accounts, saved recipe history, or durable XP storage.
- XP and your current quest reset when the page is reloaded.
- Browser support for the optional WebMCP ingredient-configuration tool varies; ordinary controls work independently of it.

For food-composition reference data, see [USDA FoodData Central](https://fdc.nal.usda.gov/). For the optional AI response format, see [OpenAI structured outputs](https://developers.openai.com/api/docs/guides/structured-outputs).

## 🛠️ Troubleshooting

| Problem | What to check |
| --- | --- |
| “Live AI recipe creation isn’t connected yet” | Add `OPENAI_API_KEY` on the server and restart or redeploy |
| AI unavailable, but recipes still appear | The fallback is working; check provider billing, model access, connectivity, and timeouts |
| No recipes found | Add a recognized main ingredient or select “Surprise me” to search every cuisine |
| A recipe says cooked rice is missing | Enter `cooked rice` only when you actually have it; dry rice is a different ingredient |
| Installation fails | Confirm the Node and pnpm versions, then install using the committed lockfile |
| The production server will not start | Run `pnpm build` before `pnpm start` |

---

<div align="center">
  <strong>Less waste. More taste. One delicious quest at a time.</strong>
</div>
