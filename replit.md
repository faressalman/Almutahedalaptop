# almutaheda laptop

متجر عربي لعرض اللابتوبات المستعملة والمجددة وحجزها من دون دفع إلكتروني.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 5000)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/almutaheda-laptop/src/App.tsx` — صفحات المتجر وتدفق التصفح والسلة والحجز التجريبي.
- `artifacts/almutaheda-laptop/src/data/products.ts` — بيانات الأجهزة التجريبية وحقولها.
- `artifacts/almutaheda-laptop/src/index.css` — تنسيق الواجهة وألوانها.
- `artifacts/api-server` و`lib/db` — خدمات مشتركة متاحة للمرحلة التي ستضيف قاعدة البيانات.

## Architecture decisions

- المرحلة الأولى نموذج واجهة يعمل ببيانات تجريبية، ولا تتصل بقاعدة بيانات أو ترسل حجوزات.
- تحفظ السلة وتعديلات عينات المنتجات محلياً للتجربة؛ لا تحفظ بيانات التواصل أو العنوان.
- العملة المعروضة USD؛ الأسعار الحالية بالدولار لكنها عينات وليست أسعار بيع فعلية.
- معاينة لوحة الإدارة عامة وتجريبية وليست محمية بعد؛ لا تعرض بيانات عملاء حقيقية.

## Product

يتيح المتجر البحث عن الأجهزة وتصفية النتائج ومقارنة المواصفات، ثم تجربة نموذج حجز يحسب عربوناً بنسبة 10% من قيمة الأجهزة ويحاكي الدفع عبر سيريتل كاش. لا يوجد اتصال بمزود دفع ولا يتم تحصيل أو تحويل أموال. الخطوة التالية هي ربط المنتجات والحجوزات بقاعدة بيانات، وإضافة دخول آمن للمدير وتنبيهات الحجوزات داخل الموقع.

## User preferences

- واجهة عربية باتجاه RTL ومتوافقة مع الجوال والكمبيوتر.
- الصورة التي أرفقها المستخدم هي الشعار الرسمي للمتجر، ويجب اعتمادها في الواجهة.
- يجب أن تتكيف القياسات والتخطيطات مع اللابتوب والآيباد والهاتف.
- أسماء اللابتوبات وموديلاتها المعروضة للمستخدم تكون باللغة الإنجليزية فقط.
- عربون الحجز 10%؛ سيريتل كاش مطلوب كخيار تجريبي فقط حالياً، من دون دفع فعلي.
- يجب ألا يتمكن الزوار من مشاهدة طلبات عملاء آخرين أو بياناتهم.
- اشرح التقدم والخطوات التالية بالعربية البسيطة؛ المستخدم مبتدئ ويريد تنفيذ التعديلات الممكنة من دون تعديل الكود يدوياً.
- المطلوب أن يصل للمدير تنبيه داخل الموقع عند تسجيل حجز فعلي.

## Gotchas

_Populate as you build — sharp edges, "always run X before Y" rules._

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
