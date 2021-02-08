### Angular test framework
**Aspects:** available components and best practices to test Angular components

**Resource:** [the official Angular documentation](https://angular.io/guide/testing)

---

### CSS
**Aspects:** different use and functionalities of flexbox layout

**Resource:** [CSS-Tricks](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)

---
### Git
**Problem:** permission denied when pushing to remote repo

**Solution:**
```
git remote rm origin
git remote add origin git@github.com…git (SSH key)
cat ~/.ssh/id_rsa.pub (view key)
https://github.com/settings/ssh (add key into GitHub account)  
```

**Resource:** [Stackoverflow](https://stackoverflow.com/questions/13674647/cannot-push-to-git-repository-permission-denied)

**Problem:** delete undesirable commit history

**Solution:**
```
Git reset --hard <SHA>
```

**Resource:** Stackoverflow

---

### GitHub

**Problem:** "refusing to allow an OAuth App to create or update workflow" on git push

**Solution:** we need to enable `workflow` scope on our GitHub repository to allow any modifications on GitHub Action in the project.
Otherwise, the request will likely be declined.

**Resource:** [Stackoverflow](https://stackoverflow.com/questions/64059610/how-to-resolve-refusing-to-allow-an-oauth-app-to-create-or-update-workflow-on)

---

### Java/OS
How to switch between Java versions on the PC for working with different project environment?

Resource: [Medium](https://medium.com/@devkosal/switching-java-jdk-versions-on-macos-80bc868e686a)
