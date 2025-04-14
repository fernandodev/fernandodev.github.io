---
layout: post
title: "Baby steps: creating seeds on NextJS"
date: 2025-04-14 20:52:16 +0200
categories: nextjs
tags: nextjs prisma
keywords: nextjs prisma seed
---

I come from a background in Ruby on Rails and there things are extremely simple, or at least I find them quite simple.

I decided then to start a new pet project using NextJS, I am obviously quite lost. So I am doing little by little, step by step, rediscovering things in a completely different environment.

Today I wanted to make a simple task: seed my DB.

<!-- more -->

## Getting Started

First of all I am assuming you have followed the NextJS initial setup and then, added the Prisma library.

So I created in my `schema.prisma` an `User` model

```
model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  name      String
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

then I've run the initial commands to generate the prisma client and the initial migration

```sh
npx prisma generate
npx prisma migrate dev --name init
```

‼️ Note that it will create under `src` a `generated` folder with the Prisma client. This is important to note as I've got stuck trying to understand why prisma client wasn't working properly.

## Seed

I've created a `seed.ts` under `prisma/` folder. And there I've setup my seed script:

```ts
import { PrismaClient } from '../src/generated/prisma'

const prisma = new PrismaClient();

async function main() {
  await prisma.user.createMany({
    data: [
      { email: 'alice@example.com', name: 'Alice' },
      { email: 'bob@example.com', name: 'Bob' },
    ],
  });
}

main()
  .then(() => {
    console.log('Seeding complete');
  })
  .catch((e) => {
    console.error('Error during seeding:', e);
    process.exit(1);
  })
  .finally(async () => {
    await prisma.$disconnect();
  });

```

‼️ Note that I am using the generated Prisma client!

## Task

Now to make the task available via CLI, it is required the ts-node library

```sh
npm install -D ts-node
```

And then finally on `package.json`

```json
"prisma": {
  "seed": "ts-node --compiler-options {\"module\":\"CommonJS\"} prisma/seed.ts"
}
```

## Finally

```
npx prisma db seed
```

And it works amazingly fine!!!

OMG HOW CRAZY IT WAS, and compared to Rails... it is an entire new world there!

You can check your user doing:

```
npx prisma studio
```

## Music of the day

<iframe style="border-radius:12px" src="https://open.spotify.com/embed/track/3CSG8VPGyLAX8uTV99a9RU?utm_source=generator" width="100%" height="120" frameBorder="0" allowfullscreen="" allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture" loading="lazy"></iframe>