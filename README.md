# MY OS v15 — Cloud Final

GitHub Pages용 iPhone-first PWA입니다. Supabase Publishable key + Anonymous Auth를 사용합니다. Secret key는 사용하지 않습니다.

## 기존 Supabase 프로젝트
기존 `MY OS` 프로젝트와 기존 테이블을 그대로 사용합니다.

## 중요: Data API 권한
2026년 Supabase의 새 public 테이블은 Data API에 자동 노출되지 않을 수 있습니다. 앱에서 `DB 접근 실패`가 나오면 Supabase SQL Editor에서 아래 권한을 한 번 실행하세요. RLS 정책은 기존 스키마의 `auth.uid() = user_id` 정책을 그대로 사용합니다.

```sql
grant select, insert, update, delete on public.profiles to authenticated;
grant select, insert, update, delete on public.tasks to authenticated;
grant select, insert, update, delete on public.captures to authenticated;
grant select, insert, update, delete on public.schedules to authenticated;
grant select, insert, update, delete on public.expenses to authenticated;
grant select, insert, update, delete on public.english_lessons to authenticated;
grant select, insert, update, delete on public.english_practice to authenticated;
grant select, insert, update, delete on public.english_diaries to authenticated;
grant select, insert, update, delete on public.diaries to authenticated;
grant select, insert, update, delete on public.insights to authenticated;
grant select, insert, update, delete on public.thoughts to authenticated;
grant select, insert, update, delete on public.projects to authenticated;
grant select, insert, update, delete on public.monthly_plans to authenticated;
grant select, insert, update, delete on public.daily_progress to authenticated;
grant select, insert, update, delete on public.rewards to authenticated;
```

Anonymous users are authenticated users after `signInAnonymously()`, so these grants apply together with RLS.

## Test
1. Open MY OS.
2. Settings → Cloud Connect.
3. Keep Project URL and `sb_publishable_...` key.
4. Tap Connect.
5. Status must change to `☁️ 클라우드 연결됨`.
6. Save a new CAPTURE and check `public.captures`.

The app now performs a real `captures` DB read before declaring cloud connectivity, and shows the actual DB error in Settings instead of silently falling back to local-only storage.
