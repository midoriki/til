# Change theme for React app using Context and SASS

Dark mode now is everywhere, I'm not a fan of it myself, but many people seem enjoy it.

There is many approach to archive theme changing for React app, this is just one of them which I choose to do.

We will use React's Context API and utilizing SASS for styling our app, so it's require you to have some basic knowledge about them.

First create the context.

```js
// themeContext.js

import React from 'react';

export const themes = {
  light: 'light',
  dark: 'dark'
};

const ThemeContext = React.createContext({
  theme: themes.light, // theme value
  setTheme: () => {} // function to change theme
});

export default ThemeContext;
```

Then put context provider to you App, or where you want your theme to be effective downward.

```js
// App.js

import React, { useState } from 'react';
import Layout from './layout/Layout';
import ThemeContext, { themes } from './themesContext';

const App = () => {
  /*
    this state is actually where the theme data is store
    we will then pass this state and it accompany function to ThemeContext
  */
  const [theme, setTheme] = useState(themes.light);

  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <Layout />
    </ThemeContext.Provider>
  )
}
```

Then at the Layout component, where theme context need to be use to render corresponding color and stuff.

```js
// layout/Layout.js

import React, { useContext } from 'react';
import './Layout.scss'; // normal styles go here
import './Layout.dark.scss'; // styles on dark mode go here

export default function Layout() {
  // useContext hook, we only get theme value
  const { theme } = useContext(ThemeContext);
  
  return (
    <div className={theme}> // the theme value is using here as class
      <div className="header">
        {..someStuff}
      </div>
      <div className="middle">
        {..someOtherStuff}
      </div>
    </div>
  )
}
```

Then, our styles for each theme mode will go to either `Layout.scss`, or `Layout.dark.scss`, my approach here is:

* Put all necessary styles to `Layout.scss`
* Put styles that special for Dark mode into `Layout.scss`, with the wrap of `.dark` around all selectors.

```scss
/* Layout.scss */

.header {
  /* all CSS that the page need for normal render */
  background: #fff;
  color: #000;
}

.middle {
  /* other CSS for difference class */
}
```

```scss
/* Layout.dark.scss */

.dark {
  .header {
    background: #000;
    color: #fff;
  }

  .middle {
    /* blah blah */
  }
}
```

The styles is too simple but you get the idea.

So when theme is `'light'`, only normal styles will be used, and when it's `'dark'`, dark styles will.

Wait, how can I change the theme ???

Let's make a `ThemeButton` component.

```js
// ThemeButton.js

import React, { useContext } from 'react';
import ThemeContext, { themes } from 'themeContext.js';

export default function ThemeButton() {
  /* you might notice here, unlike Layout.js need only theme value
    we get both theme value and setTheme function this time
    because Layout.js only use theme value, and here we need to change it
  */
  const { theme, setTheme } = useContext(ThemeContext);

  // for convenient we get other theme value here, because we have only 2 themes
  const otherTheme = Object.values(themes).find(value => value !== theme);

  const toggleTheme = () => {
    setTheme(otherTheme);
  };

  return (
    <button
      type="button"
      className="themeSwitcher"
      onClick={toggleTheme} 
    >
      Switch {otherTheme} theme
    </button>
  )
}
```

When this button is clicked, it will call the `setTheme` function of `ThemeContext`, which we created in `App.js`, and set new value.

You now can place this `ThemeButton` where ever you need.

---
Wow this is too long for a TIL. I hope someone can get the value out of this.