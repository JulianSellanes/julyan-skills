# UI Components Guide for any AI reading this

version = 2.1

## Create UI Button

There must be a single reusable button component within the project, inside a "src/components/ui" folder.
When you create a button, make sure the jsx and CSS is similar to the following example:

**Button.jsx:**

```jsx
import "./Button.css";

export const Button = ({
    className = "",
    variant = "",
    children,
    ...buttonProps
}) => {
    const cleanClassName = className?.trim()?.length ? className.trim() : "ui-bttn";
    const cleanVariantClass = variant?.trim()?.length ? ` ${variant.trim()}` : "";

    return (
        <button className={`${cleanClassName}${cleanVariantClass}`} {...buttonProps}>
            {children}
        </button>
    );
};
```

1. .ui-bttn is the default button, the one that will be on most of the page
2. className should only be used to make big changes to the button, practically a different button
3. variant should be used to make specific small changes, for example .small { font-size: 1px; }

## Create UI Icon

There must be a single reusable icon component within the project, inside a "src/components/ui" folder.
When you create an icon, make sure the jsx and CSS is similar to the following example:

**Icon.jsx:**

```jsx
import "./Icon.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import {
    faBars,
    faEye,
    faEyeSlash,
    faXmark,
} from "@fortawesome/free-solid-svg-icons";
import {
    faXTwitter,
} from "@fortawesome/free-brands-svg-icons";

const ICONS = {
    bars: faBars,
    eye: faEye,
    eyeSlash: faEyeSlash,
    xTwitter: faXTwitter,
    xmark: faXmark,
};

export const Icon = ({ name = "", className = "", ...props }) => {
    const cleanClassName = className ? ` ${className.trim()}` : "";
    const iconClassName = `ui-icon${cleanClassName}`;

    if (name === "coin") {
        return (
            <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="46 42 166 120"
                aria-hidden="true"
                className={`${iconClassName} ui-icon-svg`}
                {...props}
            >
                <path
                    fill="currentColor"
                    d="M115.656311,155.807892 C115.641228,150.331314 115.441917,145.323959 115.689850,140.338852 C115.898438,136.144836 114.491631,133.936905 110.069000,134.306732 C108.424461,134.444260 106.872879,133.246429 104.267471,134.341476 C104.267471,140.040283 104.285622,146.126083 104.262444,152.211716 C104.238556,158.482269 104.198967,158.527832 98.186302,157.336319 C92.628517,156.234955 87.262207,154.471802 81.918152,152.605652 C78.979546,151.579498 77.670471,149.804062 77.790573,146.654144 C77.949135,142.495605 77.826202,138.326141 77.816719,134.161407 C77.793640,124.027023 77.791893,124.027031 67.410164,119.925163 C66.089020,127.834473 67.789726,135.808716 66.202133,144.493759 C57.592796,139.675400 51.140163,133.539810 49.902363,124.439880 C48.086075,111.087059 47.927334,97.423668 50.127102,84.113441 C52.013706,72.698074 60.445297,65.399956 69.895241,59.545681 C83.155823,51.330700 97.768417,47.302826 113.185303,45.559090 C133.826233,43.224487 154.033844,44.704933 173.634674,51.804550 C185.691833,56.171772 196.702179,62.267170 204.546738,72.880241 C208.421097,78.121925 210.421295,83.963791 210.340515,90.570953 C210.216324,100.731453 210.411591,110.895943 210.278702,121.056221 C210.160446,130.097580 202.261078,140.803391 193.430939,143.709824 C191.889801,135.860443 193.622467,127.879929 192.298416,119.370453 C188.971848,121.227089 186.086761,122.744202 183.300568,124.425224 C181.457657,125.537125 181.616226,127.460037 181.638657,129.335236 C181.710342,135.327393 181.655487,141.321121 181.732635,147.313171 C181.757645,149.255737 181.647873,151.074371 179.572937,151.907654 C172.171555,154.879990 164.587524,157.157684 155.700378,158.136383 C155.700378,149.858856 155.700378,141.983704 155.700378,134.438675 C152.682373,133.145386 150.634903,134.544678 148.497604,134.401077 C145.226868,134.181320 144.194641,136.084076 144.247620,139.111496 C144.343796,144.607193 144.157455,150.108765 144.301224,155.602341 C144.391708,159.059906 142.902008,160.392868 139.539719,160.337662 C133.044586,160.231003 126.546555,160.280624 120.049828,160.301025 C117.315285,160.309616 115.491730,159.411835 115.656311,155.807892 M78.707489,75.200073 C67.061768,86.277016 67.723015,96.879456 80.664658,106.302650 C84.176979,108.860077 87.945152,110.992561 91.987366,112.579605 C109.405960,119.418419 127.442108,121.042282 145.841980,118.353058 C157.591766,116.635773 168.914932,113.459526 178.810760,106.407303 C192.336639,96.768143 192.472336,83.573845 179.031967,73.706329 C173.885651,69.928055 168.148773,67.238007 162.071518,65.414589 C147.442490,61.025307 132.568420,59.251133 117.295776,61.282204 C103.634384,63.098995 90.476967,66.132126 78.707489,75.200073 z"
                />
                <path
                    fill="currentColor"
                    d="M170.428085,99.433975 C161.876480,106.249557 152.034622,108.711365 141.881165,109.794960 C127.545212,111.324913 113.304794,110.800446 99.641907,105.411781 C95.759010,103.880363 92.026848,102.043716 88.946762,99.116028 C82.718079,93.195503 82.930138,85.558434 89.661552,80.175377 C96.346252,74.829666 104.300453,72.258705 112.562248,70.931969 C127.843300,68.478027 143.033966,68.890030 157.896652,73.758049 C162.034180,75.113213 165.968933,76.922287 169.376678,79.674591 C176.852890,85.712845 177.261841,92.102379 170.428085,99.433975 z"
                />
            </svg>
        );
    }

    if (name === "google") {
        return (
            <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 48 48"
                aria-hidden="true"
                className={`${iconClassName} ui-icon-svg`}
                {...props}
            >
                <path fill="#EA4335" d="M24 9.5c3.54 0 6.71 1.22 9.21 3.6l6.85-6.85C35.9 2.38 30.47 0 24 0 14.62 0 6.51 5.38 2.56 13.22l7.98 6.19C12.43 13.72 17.74 9.5 24 9.5z" />
                <path fill="#4285F4" d="M46.98 24.55c0-1.57-.15-3.09-.38-4.55H24v9.02h12.94c-.58 2.96-2.26 5.48-4.78 7.18l7.73 6c4.51-4.18 7.09-10.36 7.09-17.65z" />
                <path fill="#FBBC05" d="M10.53 28.59c-.48-1.45-.76-2.99-.76-4.59s.27-3.14.76-4.59l-7.98-6.19C.92 16.46 0 20.12 0 24c0 3.88.92 7.54 2.56 10.78l7.97-6.19z" />
                <path fill="#34A853" d="M24 48c6.48 0 11.93-2.13 15.89-5.81l-7.73-6c-2.15 1.45-4.92 2.3-8.16 2.3-6.26 0-11.57-4.22-13.47-9.91l-7.98 6.19C6.51 42.62 14.62 48 24 48z" />
                <path fill="none" d="M0 0h48v48H0z" />
            </svg>
        );
    }

    const icon = ICONS[name];
    if (!icon) return null;

    return <FontAwesomeIcon icon={icon} aria-hidden="true" className={iconClassName} {...props} />;
};
```

**Icon.css:**

```css
.ui-icon {
    flex-shrink: 0;
    vertical-align: middle;
    
    display: inline-block;

    line-height: 1;
}

.ui-icon-svg {
    width: 1.2em;
    aspect-ratio: 1 / 1;
}
```

1. In the frontend, use pnpm to import the packages:

`pnpm add @fortawesome/fontawesome-svg-core @fortawesome/free-solid-svg-icons @fortawesome/free-brands-svg-icons @fortawesome/react-fontawesome`

2) There will be icons that are not available for free, if the user requests it, ask them to give you an svg image as a reference to add the svg code

3) If this is a Nextjs project, in layout.js add:

```js
import "@fortawesome/fontawesome-svg-core/styles.css";
import { config } from "@fortawesome/fontawesome-svg-core";
config.autoAddCss = false;
```

## Create Dropdown

There must be a single reusable dropdown component within the project, inside a "src/components/ui" folder.
When you create a dropdown, make sure the jsx and CSS is similar to the following example:

**Dropdown.jsx:**

```jsx
<select id="sort" className="sort-select" value={sortBy} onChange={(e) => setSortBy(e.target.value)}>
    <option value="value1">Option1</option>
</select>
```

**Dropdown.css:**

```css
.sort-select {
    flex: 1;
    width: 100%;
    max-width: 26rem;
    min-height: 4.4rem;
    padding: 1.1rem 3.4rem 1.1rem 1.2rem;

    border: 2px solid var(--border);
    border-radius: var(--radius);

    outline: none;

    background-color: var(--bg-input);
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%23ffe066' stroke-width='3' stroke-linecap='round' stroke-linejoin='round'%3E%3Cpath d='M6 9l6 6 6-6'/%3E%3C/svg%3E");
    background-repeat: no-repeat;
    background-position: right 1.2rem center;
    background-size: 1.4rem 1.4rem;

    cursor: pointer;
    -webkit-appearance: none;
    appearance: none;

    color: var(--color);
    font-size: 1.6rem;
    font-weight: 800;
    line-height: 1.1;

    transition: border-color 0.2s ease;
}

.sort-select:focus {
    border-color: var(--gold);
}

.sort-select:disabled {
    cursor: not-allowed;
    opacity: 0.7;
}
```

## Create Footer

When you create the footer component, make sure the jsx and CSS is similar to the following example:

**Footer.jsx:**

```jsx
<footer className="footer">
    <div className="footer-bttns-group">
        <Button baseClass="footer-bttn" onClick={() => onToggle("About")}>About</Button>
        <Button baseClass="footer-bttn" onClick={() => onToggle("FAQ")}>FAQ</Button>
        <Button baseClass="footer-bttn" onClick={() => onToggle("Privacy")}>Privacy</Button>
        <Button baseClass="footer-bttn" onClick={() => onToggle("Terms")}>Terms</Button>
        <Button baseClass="footer-bttn" onClick={() => onToggle("Support")}>Support</Button>
    </div>

    <p className="footer-watermark">
        © {new Date().getFullYear()} <a href="https://julyanbuilds.com" target="_blank" rel="noopener noreferrer" className="watermark">julyanbuilds</a>
    </p>
</footer>
```

