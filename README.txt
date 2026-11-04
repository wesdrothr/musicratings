TRACKLIST MUSIC LIBRARY

Run the app:
1. Open the music-library folder.
2. Double-click index.html to open it in your web browser.

The app does not need an internet connection or any additional software.
Songs are saved in the browser storage for the browser/profile you use. Keep
using the same browser and profile to see your saved songs.

Use the Songs tab to add, edit, or delete songs. Your song list shows 20 songs
per page and is ordered from the most recently added song to the oldest. Enter the song title, one or more
artists separated by commas, length as minutes and seconds (for example, 4:17),
and a rating from 0 to 10. Use 0 as a placeholder; it is kept in the song library
but excluded from Stats, ratings averages, release ratings, artist ratings, and the
Ratings graph. Each song automatically records and displays the date
and time it was added. Editing a song does not change its original added date. Song ratings can be set in half-point increments.
Search Your songs by song title, artist, album, or EP name.
Choose a release type to classify the song as a standalone single, part of an
album, or part of an EP. When an album or EP is selected, enter its name. Use the Stats tab to see your average ratings, song count,
combined listening time, album ratings, EP ratings, singles count, and artist ratings.
The Stats tab also includes an overall average artist rating, where each artist's
individual average counts equally.
Album and EP statistics are tracked separately in their own average ratings
and breakdowns. Album, EP, and artist rating lists are ordered from highest
average rating to lowest. Artist ratings are the average song rating for each artist. Mark a song as GEEKED in the
input form, then use the Stats tab's GEEKED filter to calculate stats using
only songs marked GEEKED. Artist ratings include every artist on each song.
Album, EP, and artist rating lists show 20 entries per page and include their
own search boxes for finding a certain release or artist quickly.
Each Album, EP, and Artist entry also shows its ranking number beside the name.
Searching does not change these ranking numbers; the rank reflects the original
sorted rating list.
In Artist ratings, click an artist name to open a ranked list of all songs by
that artist, including songs where the artist is featured with other artists.
In Album ratings or EP ratings, click a release name to open its songs in input
order, without ranking them by rating. Clicked lists respect the GEEKED filter.
The Ratings tab shows a graph of how many songs have each rating from 1 to 10,
including half-point ratings. The bars grow in sequence whenever the Ratings tab is opened.
Hover over a green bar to see up to three randomly selected songs at that rating.
Searching does not change Album, EP, or Artist ranking numbers.

Phone and computer sync setup

1. In Supabase, open the SQL Editor and create one table with this SQL:

create table public.music_libraries (
  user_id uuid primary key references auth.users(id) on delete cascade,
  songs jsonb not null default '[]'::jsonb,
  updated_at timestamptz not null default now()
);

alter table public.music_libraries enable row level security;

create policy "Users can view their library"
on public.music_libraries for select
using ((select auth.uid()) = user_id);

create policy "Users can insert their library"
on public.music_libraries for insert
with check ((select auth.uid()) = user_id);

create policy "Users can update their library"
on public.music_libraries for update
using ((select auth.uid()) = user_id)
with check ((select auth.uid()) = user_id);

2. In Supabase Authentication, enable Email provider.
3. In Authentication settings, enable Email/Password auth and turn off Confirm email
   if you want account creation without sending confirmation emails.
3. Add your HTTPS website URL to Authentication Redirect URLs.
4. Deploy the music-library folder with GitHub Pages or another HTTPS host.
5. Open Tracklist, click Sign in, enter your email and password, and choose
   Sign in or Create account.
6. Sign in with the same email on your phone and computer.

Transfer existing songs

To copy an existing library to the website or another browser:
1. Open the old Tracklist page where your songs currently appear.
2. Click Export in Your songs and save the downloaded JSON file.
3. On the new Tracklist website, click Import.
4. Select the exported JSON file.
5. Confirm replacement when prompted.

Import replaces the library on the current browser. Sign in afterward to upload
the imported library to Supabase and sync it with your other devices.

GitHub Pages email sign-in

For https://wesdrothr.github.io/musicratings/, set Supabase Authentication URL
Configuration as follows:
Site URL: https://wesdrothr.github.io/musicratings/
Redirect URLs: https://wesdrothr.github.io/musicratings/
Redirect URLs: https://wesdrothr.github.io/musicratings/index.html
Redirect URLs: https://wesdrothr.github.io/musicratings/*

After changing these settings, upload the newest index.html and sw.js to GitHub,
commit them, and wait for GitHub Pages to finish publishing. Request a new email
link and click it once. Email links expire and older links may stop working.
