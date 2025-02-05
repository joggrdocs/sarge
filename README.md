# 🪖 sarge

Framework for building elegant command-line tools with Node.js &amp; TypeScript.

## Features

* 🔄 **Auto-loading**: Automatically load commands from a directory and subdirectories.
* 📝 **Configuration-based API**: Define a command and options using a simple configuration object.
* 🟦 **Typed**: Written in TypeScript, so you get full type-checking and intellisense support.
* ⚙️ **Extensible**: Easily extend the framework using the underlying `commander` library.

## Installation

**`npm`**
```bash
npm install sarge-cli
```

**`yarn`**
```bash
yarn add sarge-cli
```

## Usage

This is a simple example of how to use `sarge` to build a CLI tool, that includes:

* Pre-configured authentication using a custom provider.
* Auto-loading of commands from a directory.
* Accessing the `ProgramContext` object in a command action.

**`src/main.ts`**
```typescript
import sarge from 'sarge-cli';
import pluginRequest from '@sarge-cli/plugin-request';
import pluginAuth from '@sarge-cli/plugin-auth';
import pluginConfig from '@sarge-cli/plugin-config';

import { customProvider } from './auth-provider';

const program = sarge({
  name: 'my-cli',
  version: '1.0.0',
  description: 'My awesome CLI tool',
  
  // Auto-load commands from the `commands` directory OR specify a custom directory '/my-commands'
  autoload: true,

  // Register plugins to extend the CLI functionality 
  // and add custom implementations if needed
  plugins: [
    pluginAuth(),
    pluginRequest(),
    pluginConfig({
      files: [
        '.myclirc',
        '.myclirc.json',
        '.myclirc.yaml',
        'my-cli.config.js',
        'my-cli.config.ts',
        'random-my-cli.json',
      ],
    })
  ],
});

// returns a standard Commander object that you can use with the underlying Commander API
program.help('my-cli [command] [options]');

// parse the command-line arguments (the same as `commander.parse`)
program.parse(process.argv);
```

**`src/commands/hello.ts`**
```typescript
import { defineCommand } from 'sarge-cli';

export default defineCommand({
  name: 'hello',
  description: 'Say hello to the world',
  options: [
    {
      name: 'name',
      shortName: 'n',
      description: 'Your name',
    },
  ],
  preAction: async (ctx) => {
    await ctx.auth.check();
  },
  action: async (ctx, { options }) => {
    ctx.loader.start('Loading...');

    const user = await ctx.request.api.post('/users/search', { name: options.name });
    const other = await ctx.request.other.get('/other');

    ctx.console.log(`Hello, ${user.fullName || 'world'}!`);
    ctx.loader.success('You said hello!');
  },
});
```

**Test running the command**
```bash
sarge-cli run hello --name John OR my-cli hello -n John
```

## License

Licensed under MIT.

<br>
<hr>
<h2 align="center">

Want to sign up for Joggr? Check us out at <a href="https://joggr.io">Joggr</a>

</h2>
