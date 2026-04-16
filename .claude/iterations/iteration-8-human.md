1) rescan templates/index.json for any changes.
2) implement a new section called birdlabs-videoreels.liquid.  Look at the source code in:

/var/www/html/traceminerals_boardmansgame_com/frontend/hydrogen-frontend-v7/app/componentsMockup2/components/

VideoReels.tsx
VideoReelsIframe.tsx
VideoReelsIframe.css

3) implement the new section to mimic the display of YouTube content video reels.  
4) add configurable parameters to pull from a specific YouTube account, and a playlist.  If no playlist is specified then it pulls the latest YouTube videos for the account.
5) make the youtube account configurable.
6) the YouTube account feeding this content is to be: http://youtube.com/@buyflorabella

7) point out anything we may have missed before executing in a iteration-n-claude-questions.md
8) update CLAUDE.md to incorporate an opportunity for claude to ask questions before execution.  This is only necessary if execution is unclear.  Human answers will be inserved into claude-questions.md, and re-read for iterations.