## III. הגדרות
### שמור על ההגדרות בסביבה

הגדרות התכנה הם כל דבר שייתכן וישתנה בכל [פריסה](./codebase) שונה (בדיקות, פרודקשן, פיתוח וכד'). זה כולל:

* הגדרות לבסיס הנתונים, לזכרון המטמון [או למשאבים תומכים](./backing-services) אחרים
* פרטי הזדהות אל שירותים חיצוניים כמו אמזון S3 או טוויטר
* ערכים ייחודיים פר פריסה כמו לדוגמא שם המחשב

לעיתים תוכנות שומרות על ערכי ההגדרות בתוך הקוד. זו אינה דרכה של אפליקצייה על פי מתודולוגיית שנים-עשר הגורמים, אשר דורשת **הפרדה מלאה בין ההגדרות לבין הקוד**. הגדרות משתנות באופן מהותי בין פריסות שונות, הקוד לא.

מבחן פשוט (litmus test) לוודא האם כל ההגדרות הוגדרו בצורה נכונה היא האם ניתן להפוך את כל הפרוייקט לפרויקט קוד פתוח בכל רגע נתון, מבלי לסכן את פרטי ההתחברות שלנו.

שים לב כי הגדרה זו של "קונפיגורציה" **לא** מכלילה הגדרות פנימיות של האפליקציה, כגון `config/routes.rb` ב-Rails, או [איך מודולים מחוברים אחד לשני](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/beans.html) ב-[Spring](http://spring.io/). אלו הגדרות אשר אינן משתנות פר פריסה, ולכן כדאי להשאיר אותן בתוך הקוד.

גישה אחרת לשימוש בהגדרות היא להשתמש בקבצי אגדרות אשר אינם מוכללים במאגר הקוד שלנו, כגון `config/database.yml` ב-Rails. זהו שיפור משמעותי מאשר להשתמש בקבועים בתוך הקוד עצמו אך יש לו חסרונות: ניתן בטעות להכניס את קבצי ההגדרות למאגר הקוד; לקבצי קונגפיגורציה יש נטיה להתפזר בכל מיני מקומות ובכל מיני פורמטים שונים ונטייה זו גורמת לקושי בניהול הקבצים. יתרה מכך, הפורמט נוטה להיות שונה עבור כל שפה או סביבה.

**אפליקציות שניים-עשר הגורמים שומרות את ההגדרות *במשתנה סביבה***. ניתן לשנות משתני סביבה בצורה פשוטה בין פריסות ללא שינוי בקוד; בשונה מקבצי ההגדרות יש סיכון קטן מאוד להכניס את משתני הסביבה בטעות למאגר הקוד; ובשונה מקבצי הגדרות, או מנגנונים דומים כמו Java System Properties, הם אינם שונים בין שפות או מערכות הפעלה.

עוד נקודת מבט על ניהול ההגדרות הוא הקבצה. לעיתים מקבצים הגדרות על פי פריסות מסוימות כגון `פיתוח`, `בדיקות`, ו-`פרודקשיין`. לא ניתן להרחיב את השיטה הזו בצורה נקייה; ככל שישנם פריסות רבות יותר של האפליקציה, סביבות חדשות נוצרות ויש צורך בשמות סביבה חדשים כמו `qa`. ככל שהפרויקט גדל עוד יותר, ישנם מפתחים אשר יצטרכו סביבות משלהם כמו `בדיקות-אילן`, והתוצאה של כך היא פיצוץ של הגדרות אשר מאוד קשה לנהל אותו.

באפליקציית על פי שניים-עשר הגורמים, משתני סביבה הם גרעיניים וכל אחד נפרד לגמרי ממשתני סביבה אחרים. הם אף פעם לא יקובצו ביחד כ-"סביבה", אלא שהם מנוהלים בנפרד עבור כל פריסה. ניתן להרחיב מודל זה ללא בעיה ככל שהאפליקציה גדלה בצורה טבעית.