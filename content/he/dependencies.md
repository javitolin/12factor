## II. Dependencies
## II. תלויות
### הכרז ובודד במפורש תלויות

עבור רוב שפות התכנות קיים מנהל חבילות כמו [CPAN](http://www.cpan.org/) עבור Perl או [Rubygems](http://rubygems.org/) עבור Ruby. ניתן להתקין ספריות דרך מנהל החבילות עבור כלל המערכת או רק עבור האפליקצייה עצמה.

**אפליקצייה על פי מתודולוגיית שניים-עשר הגורמים לעולם אינה מסתמכת על חבילות שעלולות להימצא במערכת.** היא מכריזה בצורה מפורשת ומלאה על כלל התלויות שלה, על ידי קובץ *הכרזת תלויות*. יתרה מכך, היא משתמש בכלי *לבידוד תלויות* על מנת לוודא ששום תלות אינה נכנסת בצורה "אוטומטית" מהסביבה. המפרט המלאה והמפורש של התלויות מוחל הן על סביבת הפיתוח והן על סביבת הפרודקשיין.

לדוגמא, [Bundler](https://bundler.io/) עבור Ruby מנגיש קובץ `Gemfile` עבור הכרזת תלויות ו-`bundle exec` לבידוד תלויות. ב-Python יש שני כלים נפרדים לשלבים אלו --  [Pip](http://www.pip-installer.org/en/latest/) עבור הכרזת תלויות ן-[Virtualenv](http://www.virtualenv.org/en/latest/) עבור בידוד. אפילו ב-C ישנו [Autoconf](http://www.gnu.org/s/autoconf/) להכרזת תלויות ושימוש ב-Static linking יכול לגרום לבידוד תלויות. לא משנה מה השפה או הכלים, הפרדה והכרזת תלויות צריכות לבוא ביחד -- שימוש ברק אחד משני אלו לא מספיק עבור מתודולוגיית שניים-עשר הגורמים.

יתרון אחד להכרזת התלויות היא שהיא מפשטת הגדרות עבור מפתחים חדשים של האפליקציה. מפתח חדש יכול להוריד את הקוד ממאגר הקוד אליו מקומית ועליו להתקין רק את שפת התכנות ומנהל התלויות להתחיל לעבוד. הם יכולים להגדיר את מה שהם צריכים על ידי שימוש ב*פיקוד בנייה*. לדוגמא פיקוד הבנייה עבור Ruby הוא `bundle install`, בעוד שעבור Clojure/[Leiningen](https://github.com/technomancy/leiningen#readme) הוא `lein deps`.

אפליקציות על פי מתודולוגיית שניים-עשר הגורמים אינם מסתמכים על קיום כלים כלשהם במערכת. לדוגמא שימוש ב-ImageMagick או `curl`. למרות שכלים אלו לרוב קיימים במערכות, אין שום ערובה שהם יהיו קיימים בכל המערכת שהאפליקצייה תרוץ עליהן או שהגרסה שקיימת מתאימה לאפליקציה. אם האפליקציה צריכה כלי מסוים, היא צריכה להגיע איתו.