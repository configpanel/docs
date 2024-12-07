# Plugin API

## Loading plugins

```ts
import { load } from '@configpanel/plugins';
import { plugin as userPlugin, users, namespace } from '@configpanel/users';
import { plugin as forumPlugin, threads } from '@configpanel/forum';

// load plugins
load(userPlugin(), forumPlugin());

// load a plugin with custom namespace
load(userPlugin({ namespace: 'forumUsers', table: 'forumUsers' }));

(async () => {
  const users = await users.list();
  console.log(users);

  const forumUsers = await (namespace('forumUsers').users.list());

  const user = forumUsers[0];
  const userThreads = await threads.by(user);
  console.log(userThreads);
})();
```

## Creating custom plugins

```ts
import { load, type Plugin, type PluginOptions, defaultOptions, access } from '@configpanel/plugins';

export const defaultNamespace = 'example';
export const id = 'org.configpanel.example';

class MyPlugin {
  customProperty: string;

  constructor(customProperty?: string) {
    this.customProperty = customProperty ?? 'world';
  }

  hello() {
    return `Hello, ${customProperty}!`;
  }
}

interface MyPluginOptions extends PluginOptions {
  customProperty?: string;
}

export function plugin(options?: MyPluginOptions): Plugin {
  options ??= {};

  return {
    id,
    plugin: new MyPlugin(),
    ...defaultOptions(options)
  };
}

export function hello() {
  return (access(id)[defaultNamespace] as MyPlugin).hello();
}

export function namespace(namespace: string) {
  return {
    ...(access(id)[namespace] as MyPlugin);
  };
}

load(plugin(), plugin({ namespace: 'name', customProperty: 'ConfigPanel' }));

console.log(hello()); // Hello, world!
console.log(namespace('name').hello()); // Hello, ConfigPanel!
```
