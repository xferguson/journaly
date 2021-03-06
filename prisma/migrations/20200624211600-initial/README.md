# Migration `20200624211600-initial`

This migration has been generated by Ryan "Lanny" Jenkins at 6/24/2020, 9:16:00 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
CREATE TYPE "UserRole" AS ENUM ('ADMIN', 'MODERATOR', 'FREE_USER', 'PRO_USER');

CREATE TYPE "PostStatus" AS ENUM ('DRAFT', 'PUBLISHED');

CREATE TYPE "ImageRole" AS ENUM ('HEADLINE', 'INLINE');

CREATE TYPE "SocialMediaPlatform" AS ENUM ('FACEBOOK', 'YOUTUBE', 'INSTAGRAM', 'LINKEDIN');

CREATE TABLE "public"."User" (
"bio" text   ,"createdAt" timestamp(3)  NOT NULL DEFAULT CURRENT_TIMESTAMP,"email" text  NOT NULL ,"handle" text  NOT NULL ,"id" SERIAL,"locationId" integer   ,"name" text   ,"profileImage" text   ,"updatedAt" timestamp(3)  NOT NULL ,"userRole" "UserRole" NOT NULL DEFAULT 'FREE_USER',
    PRIMARY KEY ("id"))

CREATE TABLE "public"."Auth" (
"id" SERIAL,"password" text  NOT NULL ,"resetToken" text   ,"resetTokenExpiry" timestamp(3)   ,"userId" integer  NOT NULL ,
    PRIMARY KEY ("id"))

CREATE TABLE "public"."LanguageLearning" (
"createdAt" timestamp(3)  NOT NULL DEFAULT CURRENT_TIMESTAMP,"id" SERIAL,"languageId" integer  NOT NULL ,"userId" integer  NOT NULL ,
    PRIMARY KEY ("id"))

CREATE TABLE "public"."LanguageNative" (
"createdAt" timestamp(3)  NOT NULL DEFAULT CURRENT_TIMESTAMP,"id" SERIAL,"languageId" integer  NOT NULL ,"userId" integer  NOT NULL ,
    PRIMARY KEY ("id"))

CREATE TABLE "public"."SocialMedia" (
"authorId" integer  NOT NULL ,"id" SERIAL,"platform" "SocialMediaPlatform" NOT NULL ,"url" text  NOT NULL ,
    PRIMARY KEY ("id"))

CREATE TABLE "public"."Post" (
"authorId" integer  NOT NULL ,"body" text  NOT NULL ,"bodySrc" text  NOT NULL DEFAULT '',"createdAt" timestamp(3)  NOT NULL DEFAULT CURRENT_TIMESTAMP,"excerpt" text  NOT NULL ,"id" SERIAL,"languageId" integer  NOT NULL ,"latitude" Decimal(65,30)   ,"longitude" Decimal(65,30)   ,"readTime" integer  NOT NULL DEFAULT 1,"status" "PostStatus" NOT NULL DEFAULT 'DRAFT',"title" text  NOT NULL ,"updatedAt" timestamp(3)  NOT NULL ,
    PRIMARY KEY ("id"))

CREATE TABLE "public"."Language" (
"dialect" text   ,"id" SERIAL,"name" text  NOT NULL ,
    PRIMARY KEY ("id"))

CREATE TABLE "public"."Location" (
"city" text  NOT NULL ,"country" text  NOT NULL ,"id" SERIAL,
    PRIMARY KEY ("id"))

CREATE TABLE "public"."Topic" (
"id" SERIAL,"name" text  NOT NULL ,"userId" integer   ,
    PRIMARY KEY ("id"))

CREATE TABLE "public"."UserInterests" (
"createdAt" timestamp(3)  NOT NULL DEFAULT CURRENT_TIMESTAMP,"id" SERIAL,"topicId" integer  NOT NULL ,"userId" integer  NOT NULL ,
    PRIMARY KEY ("id"))

CREATE TABLE "public"."PostTopics" (
"id" SERIAL,"postId" integer  NOT NULL ,"topicId" integer  NOT NULL ,
    PRIMARY KEY ("id"))

CREATE TABLE "public"."PostLike" (
"createdAt" timestamp(3)  NOT NULL DEFAULT CURRENT_TIMESTAMP,"id" SERIAL,"postId" integer  NOT NULL ,"userId" integer  NOT NULL ,
    PRIMARY KEY ("id"))

CREATE TABLE "public"."CommentLike" (
"commentId" integer  NOT NULL ,"createdAt" timestamp(3)  NOT NULL DEFAULT CURRENT_TIMESTAMP,"id" SERIAL,"userId" integer  NOT NULL ,
    PRIMARY KEY ("id"))

CREATE TABLE "public"."Comment" (
"authorId" integer  NOT NULL ,"body" text  NOT NULL ,"createdAt" timestamp(3)  NOT NULL DEFAULT CURRENT_TIMESTAMP,"id" SERIAL,"threadId" integer  NOT NULL ,"updatedAt" timestamp(3)  NOT NULL ,
    PRIMARY KEY ("id"))

CREATE TABLE "public"."Thread" (
"endIndex" integer  NOT NULL ,"highlightedContent" text  NOT NULL ,"id" SERIAL,"postId" integer  NOT NULL ,"startIndex" integer  NOT NULL ,
    PRIMARY KEY ("id"))

CREATE TABLE "public"."Image" (
"id" SERIAL,"imageRole" "ImageRole" NOT NULL ,"largeSize" text  NOT NULL ,"postId" integer   ,"smallSize" text  NOT NULL ,
    PRIMARY KEY ("id"))

CREATE TABLE "public"."Prompt" (
"id" SERIAL,"text" text  NOT NULL ,"topicId" integer  NOT NULL ,
    PRIMARY KEY ("id"))

CREATE TABLE "public"."UserSubscription" (
"createdAt" timestamp(3)  NOT NULL DEFAULT CURRENT_TIMESTAMP,"id" SERIAL,"name" text  NOT NULL ,"price" integer  NOT NULL ,"updatedAt" timestamp(3)  NOT NULL ,"userId" integer  NOT NULL ,
    PRIMARY KEY ("id"))

CREATE TABLE "public"."PageView" (
"id" SERIAL,"pageName" text  NOT NULL ,"sessionId" text  NOT NULL ,"timestamp" timestamp(3)  NOT NULL DEFAULT CURRENT_TIMESTAMP,"userId" integer   ,"user_agent_language" text  NOT NULL ,"user_agent_string" text  NOT NULL ,"utmSource" text   ,
    PRIMARY KEY ("id"))

CREATE TABLE "public"."_PostToTopic" (
"A" integer  NOT NULL ,"B" integer  NOT NULL )

CREATE UNIQUE INDEX "User.email" ON "public"."User"("email")

CREATE UNIQUE INDEX "User.handle" ON "public"."User"("handle")

CREATE UNIQUE INDEX "Auth.userId" ON "public"."Auth"("userId")

CREATE UNIQUE INDEX "LanguageLearning.userId" ON "public"."LanguageLearning"("userId")

CREATE UNIQUE INDEX "LanguageLearning.userId_languageId" ON "public"."LanguageLearning"("userId","languageId")

CREATE UNIQUE INDEX "LanguageNative.userId_languageId" ON "public"."LanguageNative"("userId","languageId")

CREATE UNIQUE INDEX "Language.name_dialect" ON "public"."Language"("name","dialect")

CREATE UNIQUE INDEX "UserInterests.userId_topicId" ON "public"."UserInterests"("userId","topicId")

CREATE UNIQUE INDEX "PostTopics.postId_topicId" ON "public"."PostTopics"("postId","topicId")

CREATE UNIQUE INDEX "UserSubscription_userId" ON "public"."UserSubscription"("userId")

CREATE UNIQUE INDEX "_PostToTopic_AB_unique" ON "public"."_PostToTopic"("A","B")

CREATE  INDEX "_PostToTopic_B_index" ON "public"."_PostToTopic"("B")

ALTER TABLE "public"."User" ADD FOREIGN KEY ("locationId")REFERENCES "public"."Location"("id") ON DELETE SET NULL  ON UPDATE CASCADE

ALTER TABLE "public"."Auth" ADD FOREIGN KEY ("userId")REFERENCES "public"."User"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."LanguageLearning" ADD FOREIGN KEY ("userId")REFERENCES "public"."User"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."LanguageLearning" ADD FOREIGN KEY ("languageId")REFERENCES "public"."Language"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."LanguageNative" ADD FOREIGN KEY ("userId")REFERENCES "public"."User"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."LanguageNative" ADD FOREIGN KEY ("languageId")REFERENCES "public"."Language"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."SocialMedia" ADD FOREIGN KEY ("authorId")REFERENCES "public"."User"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."Post" ADD FOREIGN KEY ("authorId")REFERENCES "public"."User"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."Post" ADD FOREIGN KEY ("languageId")REFERENCES "public"."Language"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."Topic" ADD FOREIGN KEY ("userId")REFERENCES "public"."User"("id") ON DELETE SET NULL  ON UPDATE CASCADE

ALTER TABLE "public"."UserInterests" ADD FOREIGN KEY ("userId")REFERENCES "public"."User"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."UserInterests" ADD FOREIGN KEY ("topicId")REFERENCES "public"."Topic"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."PostTopics" ADD FOREIGN KEY ("postId")REFERENCES "public"."Post"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."PostTopics" ADD FOREIGN KEY ("topicId")REFERENCES "public"."Topic"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."PostLike" ADD FOREIGN KEY ("userId")REFERENCES "public"."User"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."PostLike" ADD FOREIGN KEY ("postId")REFERENCES "public"."Post"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."CommentLike" ADD FOREIGN KEY ("userId")REFERENCES "public"."User"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."CommentLike" ADD FOREIGN KEY ("commentId")REFERENCES "public"."Comment"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."Comment" ADD FOREIGN KEY ("authorId")REFERENCES "public"."User"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."Comment" ADD FOREIGN KEY ("threadId")REFERENCES "public"."Thread"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."Thread" ADD FOREIGN KEY ("postId")REFERENCES "public"."Post"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."Image" ADD FOREIGN KEY ("postId")REFERENCES "public"."Post"("id") ON DELETE SET NULL  ON UPDATE CASCADE

ALTER TABLE "public"."Prompt" ADD FOREIGN KEY ("topicId")REFERENCES "public"."Topic"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."UserSubscription" ADD FOREIGN KEY ("userId")REFERENCES "public"."User"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."PageView" ADD FOREIGN KEY ("userId")REFERENCES "public"."User"("id") ON DELETE SET NULL  ON UPDATE CASCADE

ALTER TABLE "public"."_PostToTopic" ADD FOREIGN KEY ("A")REFERENCES "public"."Post"("id") ON DELETE CASCADE  ON UPDATE CASCADE

ALTER TABLE "public"."_PostToTopic" ADD FOREIGN KEY ("B")REFERENCES "public"."Topic"("id") ON DELETE CASCADE  ON UPDATE CASCADE
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration ..20200624211600-initial
--- datamodel.dml
+++ datamodel.dml
@@ -1,0 +1,249 @@
+datasource db {
+  provider = "postgresql"
+  url      = env("DATABASE_URL")
+}
+
+generator prisma_client {
+  provider = "prisma-client-js"
+}
+
+model User {
+  id                Int                @default(autoincrement()) @id
+  name              String?
+  email             String             @unique
+  handle            String             @unique
+  auth              Auth?
+  userRole          UserRole           @default(FREE_USER)
+  bio               String?
+  posts             Post[]
+  subscription      UserSubscription?
+  socialMedia       SocialMedia[]
+  interests         Topic[]
+  location          Location?          @relation(fields: [locationId], references: [id])
+  locationId        Int?
+  languagesNative   LanguageNative[]
+  languagesLearning LanguageLearning[]
+  createdAt         DateTime           @default(now())
+  updatedAt         DateTime           @updatedAt
+  userInterests     UserInterests[]
+  postLike          PostLike[]
+  commentLike       CommentLike[]
+  pageView          PageView[]
+  profileImage      String?
+  comment           Comment[]
+}
+
+model Auth {
+  id               Int       @default(autoincrement()) @id
+  user             User      @relation(fields: [userId], references: [id])
+  userId           Int       @unique
+  password         String
+  resetToken       String?
+  resetTokenExpiry DateTime?
+}
+
+model LanguageLearning {
+  id         Int      @default(autoincrement()) @id
+  user       User     @relation(fields: [userId], references: [id])
+  userId     Int      @unique
+  language   Language @relation(fields: [languageId], references: [id])
+  languageId Int
+  createdAt  DateTime @default(now())
+
+  @@unique([userId, languageId])
+}
+
+model LanguageNative {
+  id         Int      @default(autoincrement()) @id
+  user       User     @relation(fields: [userId], references: [id])
+  userId     Int
+  language   Language @relation(fields: [languageId], references: [id])
+  languageId Int
+  createdAt  DateTime @default(now())
+
+  @@unique([userId, languageId])
+}
+
+model SocialMedia {
+  id       Int                 @default(autoincrement()) @id
+  platform SocialMediaPlatform
+  url      String
+  author   User                @relation(fields: [authorId], references: [id])
+  authorId Int
+}
+
+model Post {
+  id         Int          @default(autoincrement()) @id
+  title      String
+  body       String
+  bodySrc    String       @default("")
+  excerpt    String
+  status     PostStatus   @default(DRAFT)
+  images     Image[]
+  createdAt  DateTime     @default(now())
+  updatedAt  DateTime     @updatedAt
+  author     User         @relation(fields: [authorId], references: [id])
+  authorId   Int
+  readTime   Int          @default(1)
+  likes      PostLike[]
+  topic      Topic[]      @relation(references: [id])
+  language   Language     @relation(fields: [languageId], references: [id])
+  languageId Int
+  longitude  Float?
+  latitude   Float?
+  threads    Thread[]
+  postTopics PostTopics[]
+}
+
+model Language {
+  id            Int                @default(autoincrement()) @id
+  posts         Post[]
+  name          String
+  dialect       String?
+  nativeUsers   LanguageNative[]
+  learningUsers LanguageLearning[]
+
+  @@unique([name, dialect])
+}
+
+model Location {
+  id      Int    @default(autoincrement()) @id
+  country String
+  city    String
+  User    User[]
+}
+
+model Topic {
+  id            Int             @default(autoincrement()) @id
+  name          String
+  posts         Post[]          @relation(references: [id])
+  user          User?           @relation(fields: [userId], references: [id])
+  userId        Int?
+  userInterests UserInterests[]
+  postTopics    PostTopics[]
+  prompt        Prompt[]
+}
+
+model UserInterests {
+  id        Int      @default(autoincrement()) @id
+  user      User     @relation(fields: [userId], references: [id])
+  userId    Int
+  topic     Topic    @relation(fields: [topicId], references: [id])
+  topicId   Int
+  createdAt DateTime @default(now())
+
+  @@unique([userId, topicId])
+}
+
+model PostTopics {
+  id      Int   @default(autoincrement()) @id
+  post    Post  @relation(fields: [postId], references: [id])
+  postId  Int
+  topic   Topic @relation(fields: [topicId], references: [id])
+  topicId Int
+
+  @@unique([postId, topicId])
+}
+
+model PostLike {
+  id        Int      @default(autoincrement()) @id
+  createdAt DateTime @default(now())
+  author    User     @relation(fields: [userId], references: [id])
+  userId    Int
+  post      Post     @relation(fields: [postId], references: [id])
+  postId    Int
+}
+
+model CommentLike {
+  id        Int      @default(autoincrement()) @id
+  createdAt DateTime @default(now())
+  author    User     @relation(fields: [userId], references: [id])
+  userId    Int
+  comment   Comment  @relation(fields: [commentId], references: [id])
+  commentId Int
+}
+
+model Comment {
+  id        Int           @default(autoincrement()) @id
+  createdAt DateTime      @default(now())
+  updatedAt DateTime      @updatedAt
+  body      String
+  author    User          @relation(fields: [authorId], references: [id])
+  authorId  Int
+  thread    Thread        @relation(fields: [threadId], references: [id])
+  threadId  Int
+  likes     CommentLike[]
+}
+
+model Thread {
+  id                 Int       @default(autoincrement()) @id
+  startIndex         Int
+  endIndex           Int
+  highlightedContent String
+  post               Post      @relation(fields: [postId], references: [id])
+  postId             Int
+  comments           Comment[]
+}
+
+model Image {
+  id        Int       @default(autoincrement()) @id
+  smallSize String
+  largeSize String
+  imageRole ImageRole
+  post      Post?     @relation(fields: [postId], references: [id])
+  postId    Int?
+}
+
+model Prompt {
+  id      Int    @default(autoincrement()) @id
+  text    String
+  topic   Topic  @relation(fields: [topicId], references: [id])
+  topicId Int
+}
+
+model UserSubscription {
+  id        Int      @default(autoincrement()) @id
+  name      String
+  price     Int
+  user      User     @relation(fields: [userId], references: [id])
+  userId    Int
+  createdAt DateTime @default(now())
+  updatedAt DateTime @updatedAt
+}
+
+model PageView {
+  id                  Int      @default(autoincrement()) @id
+  user                User?    @relation(fields: [userId], references: [id])
+  userId              Int?
+  sessionId           String
+  pageName            String
+  timestamp           DateTime @default(now())
+  utmSource           String?
+  // TODO (robin-macpherson): find user agent string parsing library and store each part separately
+  user_agent_string   String
+  user_agent_language String
+}
+
+enum UserRole {
+  ADMIN
+  MODERATOR
+  FREE_USER
+  PRO_USER
+}
+
+enum PostStatus {
+  DRAFT
+  PUBLISHED
+}
+
+enum ImageRole {
+  HEADLINE
+  INLINE
+}
+
+enum SocialMediaPlatform {
+  FACEBOOK
+  YOUTUBE
+  INSTAGRAM
+  LINKEDIN
+}
```
