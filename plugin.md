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
