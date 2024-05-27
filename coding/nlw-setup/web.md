# FRONTEND
## [Vite:](https://vitejs.dev/) Development environment tooling.
```powershell
npm create vite@latest
```

## Styling

### [tailwindcss](https://tailwindcss.com/) _Utility first CSS framework_

**Installing with PostCSS, [docs](https://tailwindcss.com/docs/installation/using-postcss):**
```PowerShell
npm install -D tailwind postcss autoprefixer
```

**Then _(-p creates postcss.config.cjs file as well)_:**
```PowerShell
npx tailwindcss init -p
```

**Setting up _./postcss.config.cjs_:**
```js
module.exports = {
	plugins: {
		tailwindcss: {},
	    autoprefixer: {},
	},
}
```

**Setting up _./tailwind.config.cjs_:**
```js
/** @type {import('tailwindcss').Config} */
module.exports = { 
	content: ["./src/**/*.tsx", "./index.html"],
	theme: { 
		extend: {},
	 },
	plugins: [], 
}
```
**TAKE NOTE:** 
 - _content:_ defines files gonna be stylized by tailwindcss
 - _theme/extend_: extends any customized stylization necessary

**Setting up _./styles/global.css_:**
```css
@tailwind base;
@tailwind utilities;
@tailwind components;
```



## Accessibility
### [Radix UI](https://www.radix-ui.com/docs/primitives/overview/introduction)
Radix Primitives is a low-level UI component library with a focus on accessibility, customization and developer experience. You can use these components either as the base layer of your design system, or adopt them incrementally.

It's possible install only the elements necessary, not the whole library.

#### [Dialog](https://www.radix-ui.com/docs/primitives/components/dialog)
```powershell
npm install @radix-ui/react-dialog
```

#### [Popover](https://www.radix-ui.com/docs/primitives/components/popover)
```powershell
npm install @radix-ui/react-popover
```
**TO DO:**
- [ ] Replace progress bar with radix ui progress bar component

## clsx 
A lib to control statements in class names when styling with tailwind.
```tsx
<Popover.Trigger
	className={clsx(
	  "w-10 h-10 bg-zinc-900 border-2 border-zinc-800 rounded-lg",
	  {
		"bg-violet-500 border-violet-400": percentageCompletion >= 80,
		
		"bg-violet-600 border-violet-500":
		  percentageCompletion >= 60 && percentageCompletion < 80,
		  
		"bg-violet-700 border-violet-500":
		  percentageCompletion >= 40 && percentageCompletion < 60,
		  
		"bg-violet-800 border-violet-600":
		  percentageCompletion >= 20 && percentageCompletion < 40,
		  
		"bg-violet-900 border-violet-700":
		  percentageCompletion > 0 && percentageCompletion < 20,
		  
		"bg-zinc-900 border-zinc-800": percentageCompletion === 0,
	  }
	)}
/>
```

## axios
- `lib/axios.ts`
```ts
import axios from "axios";
 
export const api = axios.create({
  baseURL: "http://localhost:3333",
});
```

- fetching:
```tsx
type Summary = {
  id: string;
  date: string;
  completed: number;
  total: number;
}[];

export function SummaryTable() {
  const [summary, setSummary] = useState<Summary>([]);

  useEffect(() => {
    api.get("summary").then((response) => {
      setSummary(response.data);
    });
}, []);
```

## next level
auth (firebase, auth0)
	database por usuário

push notification / workers servies
perfil publico com gráfico de resumo
