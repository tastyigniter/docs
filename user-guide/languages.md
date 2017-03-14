---
layout: page
section: user-guide
title: Languages
---
Create new language folders in /admin/language/ and /main/language.

Each language folder should be named the same in both admin and main language folders.

Example: /admin/language/chinese and /main/language/chinese

Copy the contents of the default English folders /admin/language/english and /main/language/english, or download your translation from https://crowdin.com/project/tastyigniter and copy the folder contents to their respective folders.

Change your file permissions according to your installation and then add your language under the Localisation/Languages menu in TastyIgniter admin panel. Select new and fill out the details. The Idiom field should be the same name as your language folder you added in /admin/language and /main/language.

Example: chinese

This matches /admin/language/chinese and /main/language/chinese (Note that naming is case sensitive).

You'll need to create your own language switcher.

{{site.data.alerts.callout_warning}} This section is incomplete. Please help to improve it.{{site.data.alerts.end}}