<p align="center">
  <img src="https://github.com/user-attachments/assets/774b5fbd-a1e8-4912-9b2a-b852af65e0d2" alt="TODO App screenshot Svelte">
</p>

# Demo TODO App fullstack: Svelte + Manifest 🧡🦚

This repository showcases a demo of a small **todo application** combining [Svelte](https://svelte.dev/) as a frontend and [Manifest](https://manifest.build) as backend.

The backend consists in only a few lines of code:

```yaml
name: My TODO App ✅
entities:
  Todo:
    seedCount: 10
    properties:
      - title
      - { name: completed, type: boolean }
```


## Install

```bash
npm install

# Launch backend
npm run manifest

# Launch frontend
npm run dev
```
