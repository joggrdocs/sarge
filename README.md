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

import { customProvider } from './auth-provider';

const program = sarge({
  name: 'my-cli',
  version: '1.0.0',
  description: 'My awesome CLI tool',
  auth: {
    strategy: 'web',
    provider: customProvider,
    url: 'https://example.com/auth',
  },
  autoload: true,
  configs: [
    '.myclirc',
    '.myclirc.json',
    '.myclirc.yaml',
    'my-cli.config.js',
    'my-cli.config.ts',
    'random-my-cli.json',
  ]
});

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
  action: (ctx, { options }) => {
    ctx.loader.start('Loading...');
    ctx.console.log(`Hello, ${options.name || 'world'}!`);
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
    Want to sign up for Joggr?
</h2>
<p align="center">
    We'd love to have you join, but we are in closed beta. <br> You can join our waitlist below.
</p>
<p align="center">
  <a href="https://www.joggr.io/waitlist?utm_source=github&utm_medium=org-readme&utm_campaign=static-docs">
    <img src="https://cdn.joggr.io/assets/static/badges/joggr-waitlist.svg" width="250px" alt="Join the Waitlist" />
  </a>
</p>
