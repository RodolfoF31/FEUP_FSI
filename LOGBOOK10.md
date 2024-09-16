# SEED Labs – Secret-Key Encryption Lab 

## Task 1: Frequency Analysis  

In this assignment, our objective is to decipher an encrypted message that has undergone a letter substitution method. Each letter in the message has been replaced by another, with each letter corresponding to a specific substitute. They provided us with a Python script designed to analyze character frequencies, allowing us to decrypt the text. This approach relies on identifying the most common letters in the English language, aiding us in the deciphering process.

After conducting various tests and character substitutions, we have arrived at the following key: 

```python
key = {
    'a': 'c',
    'b': 'f',
    'c': 'm',
    'd': 'y',
    'e': 'p',
    'f': 'v',
    'g': 'b',
    'h': 'r',
    'i': 'l',
    'j': 'q',
    'k': 'x',
    'l': 'w',
    'm': 'i',
    'n': 'e',
    'o': 'j',
    'p': 'd',
    'q': 's',
    'r': 'g',
    's': 'k',
    't': 'h',
    'u': 'n',
    'v': 'a',
    'w': 'z',
    'x': 'o',
    'y': 't',
    'z': 'u'
}
```

With this key, we have successfully decrypted the message, revealing its content to be: 

<details>
<summary>THE OSCARS TURN  ON SUNDAY WHICH SEEMS ABOUT RIGHT AFTER THIS LONG STRANGE
AWARDS TRIP THE BAGGER FEELS LIKE A NONAGENARIAN TOO</summary>

THE AWARDS RACE WAS BOOKENDED BY THE DEMISE OF HARVEY WEINSTEIN AT ITS OUTSET
AND THE APPARENT IMPLOSION OF HIS FILM COMPANY AT THE END AND IT WAS SHAPED BY
THE EMERGENCE OF METOO TIMES UP BLACKGOWN POLITICS ARMCANDY ACTIVISM AND
A NATIONAL CONVERSATION AS BRIEF AND MAD AS A FEVER DREAM ABOUT WHETHER THERE
OUGHT TO BE A PRESIDENT WINFREY THE SEASON DIDNT JUST SEEM EXTRA LONG IT WAS
EXTRA LONG BECAUSE THE OSCARS WERE MOVED TO THE FIRST WEEKEND IN MARCH TO
AVOID CONFLICTING WITH THE CLOSING CEREMONY OF THE WINTER OLYMPICS THANKS
PYEONGCHANG

ONE BIG QUESTION SURROUNDING THIS YEARS ACADEMY AWARDS IS HOW OR IF THE
CEREMONY WILL ADDRESS METOO ESPECIALLY AFTER THE GOLDEN GLOBES WHICH BECAME
A JUBILANT COMINGOUT PARTY FOR TIMES UP THE MOVEMENT SPEARHEADED BY 
POWERFUL HOLLYWOOD WOMEN WHO HELPED RAISE MILLIONS OF DOLLARS TO FIGHT SEXUAL
HARASSMENT AROUND THE COUNTRY

SIGNALING THEIR SUPPORT GOLDEN GLOBES ATTENDEES SWATHED THEMSELVES IN BLACK
SPORTED LAPEL PINS AND SOUNDED OFF ABOUT SEXIST POWER IMBALANCES FROM THE RED
CARPET AND THE STAGE ON THE AIR E WAS CALLED OUT ABOUT PAY INEQUITY AFTER
ITS FORMER ANCHOR CATT SADLER QUIT ONCE SHE LEARNED THAT SHE WAS MAKING FAR
LESS THAN A MALE COHOST AND DURING THE CEREMONY NATALIE PORTMAN TOOK A BLUNT
AND SATISFYING DIG AT THE ALLMALE ROSTER OF NOMINATED DIRECTORS HOW COULD
THAT BE TOPPED

AS IT TURNS OUT AT LEAST IN TERMS OF THE OSCARS IT PROBABLY WONT BE

WOMEN INVOLVED IN TIMES UP SAID THAT ALTHOUGH THE GLOBES SIGNIFIED THE
INITIATIVES LAUNCH THEY NEVER INTENDED IT TO BE JUST AN AWARDS SEASON
CAMPAIGN OR ONE THAT BECAME ASSOCIATED ONLY WITH REDCARPET ACTIONS INSTEAD
A SPOKESWOMAN SAID THE GROUP IS WORKING BEHIND CLOSED DOORS AND HAS SINCE
AMASSED  MILLION FOR ITS LEGAL DEFENSE FUND WHICH AFTER THE GLOBES WAS
FLOODED WITH THOUSANDS OF DONATIONS OF  OR LESS FROM PEOPLE IN SOME 
COUNTRIES


NO CALL TO WEAR BLACK GOWNS WENT OUT IN ADVANCE OF THE OSCARS THOUGH THE
MOVEMENT WILL ALMOST CERTAINLY BE REFERENCED BEFORE AND DURING THE CEREMONY 
ESPECIALLY SINCE VOCAL METOO SUPPORTERS LIKE ASHLEY JUDD LAURA DERN AND
NICOLE KIDMAN ARE SCHEDULED PRESENTERS

ANOTHER FEATURE OF THIS SEASON NO ONE REALLY KNOWS WHO IS GOING TO WIN BEST
PICTURE ARGUABLY THIS HAPPENS A LOT OF THE TIME INARGUABLY THE NAILBITER
NARRATIVE ONLY SERVES THE AWARDS HYPE MACHINE BUT OFTEN THE PEOPLE FORECASTING
THE RACE SOCALLED OSCAROLOGISTS CAN MAKE ONLY EDUCATED GUESSES

THE WAY THE ACADEMY TABULATES THE BIG WINNER DOESNT HELP IN EVERY OTHER
CATEGORY THE NOMINEE WITH THE MOST VOTES WINS BUT IN THE BEST PICTURE
CATEGORY VOTERS ARE ASKED TO LIST THEIR TOP MOVIES IN PREFERENTIAL ORDER IF A
MOVIE GETS MORE THAN  PERCENT OF THE FIRSTPLACE VOTES IT WINS WHEN NO
MOVIE MANAGES THAT THE ONE WITH THE FEWEST FIRSTPLACE VOTES IS ELIMINATED AND
ITS VOTES ARE REDISTRIBUTED TO THE MOVIES THAT GARNERED THE ELIMINATED BALLOTS
SECONDPLACE VOTES AND THIS CONTINUES UNTIL A WINNER EMERGES

IT IS ALL TERRIBLY CONFUSING BUT APPARENTLY THE CONSENSUS FAVORITE COMES OUT
AHEAD IN THE END THIS MEANS THAT ENDOFSEASON AWARDS CHATTER INVARIABLY
INVOLVES TORTURED SPECULATION ABOUT WHICH FILM WOULD MOST LIKELY BE VOTERS
SECOND OR THIRD FAVORITE AND THEN EQUALLY TORTURED CONCLUSIONS ABOUT WHICH
FILM MIGHT PREVAIL

IN  IT WAS A TOSSUP BETWEEN BOYHOOD AND THE EVENTUAL WINNER BIRDMAN
IN  WITH LOTS OF EXPERTS BETTING ON THE REVENANT OR THE BIG SHORT THE
PRIZE WENT TO SPOTLIGHT LAST YEAR NEARLY ALL THE FORECASTERS DECLARED LA
LA LAND THE PRESUMPTIVE WINNER AND FOR TWO AND A HALF MINUTES THEY WERE
CORRECT BEFORE AN ENVELOPE SNAFU WAS REVEALED AND THE RIGHTFUL WINNER
MOONLIGHT WAS CROWNED

THIS YEAR AWARDS WATCHERS ARE UNEQUALLY DIVIDED BETWEEN THREE BILLBOARDS
OUTSIDE EBBING MISSOURI THE FAVORITE AND THE SHAPE OF WATER WHICH IS
THE BAGGERS PREDICTION WITH A FEW FORECASTING A HAIL MARY WIN FOR GET OUT

BUT ALL OF THOSE FILMS HAVE HISTORICAL OSCARVOTING PATTERNS AGAINST THEM THE
SHAPE OF WATER HAS  NOMINATIONS MORE THAN ANY OTHER FILM AND WAS ALSO
NAMED THE YEARS BEST BY THE PRODUCERS AND DIRECTORS GUILDS YET IT WAS NOT
NOMINATED FOR A SCREEN ACTORS GUILD AWARD FOR BEST ENSEMBLE AND NO FILM HAS
WON BEST PICTURE WITHOUT PREVIOUSLY LANDING AT LEAST THE ACTORS NOMINATION
SINCE BRAVEHEART IN  THIS YEAR THE BEST ENSEMBLE SAG ENDED UP GOING TO
THREE BILLBOARDS WHICH IS SIGNIFICANT BECAUSE ACTORS MAKE UP THE ACADEMYS
LARGEST BRANCH THAT FILM WHILE DIVISIVE ALSO WON THE BEST DRAMA GOLDEN GLOBE
AND THE BAFTA BUT ITS FILMMAKER MARTIN MCDONAGH WAS NOT NOMINATED FOR BEST
DIRECTOR AND APART FROM ARGO MOVIES THAT LAND BEST PICTURE WITHOUT ALSO
EARNING BEST DIRECTOR NOMINATIONS ARE FEW AND FAR BETWEEN
</details>

##  Task 2: Encryption using Different Ciphers and Modes

In this task we are asked to play a little with different types of encryption with the use of the line in the shell: 
```bash
openssl enc -ciphertype -e -in plain.txt -out cipher.bin \
-K 00112233445566778889aabbccddeeff \
-iv 0102030405060708
```

### Encryption Algorithm Examples
We have been provided with three examples of encryption algorithms, each with its unique characteristics:

#### AES-128-CBC

AES (Advanced Encryption Standard) with a 128-bit key operates in CBC (Cipher Block Chaining) mode. In CBC mode, each plaintext block is XORed with the previous ciphertext block before encryption. This process enhances security by making it challenging to recognize any patterns in the encrypted message.

#### BF-CBC

BF (Blowfish) is a lesser-used encryption algorithm featuring a variable key size ranging from 32 to 448 bits. Utilizing CBC mode as described earlier, Blowfish offers flexibility in key size and is known for its relatively fast encryption and decryption processes.

#### AES-128-CFB

Similar to the first algorithm, AES-128-CFB operates in CFB (Cipher Feedback) mode. CFB mode allows for the encryption of individual bits, making it suitable for stream cipher applications. This mode provides adaptability for scenarios requiring encryption at the bit level.

When selecting an encryption algorithm, considerations such as security requirements, key size flexibility, and performance characteristics are crucial for meeting specific use case needs.

# Task 3: Encryption Mode – ECB vs. CBC

For this task, the objective is to encrypt an image in a basic format, specifically in .bmp. It's emphasized that the initial 54 bytes in this format are crucial for proper image recognition. Consequently, the first 54 bytes of the final image must remain unencrypted to ensure successful opening. To safeguard against potential image 'corruption,' the following approach can be employed:

```bash
$ head -c 54 p1.bmp > header
$ tail -c +55 p2.bmp > body
$ cat header body > new.bmp
```

Essentially, this process involves extracting the initial 54 bytes from the original image (p1.bmp) and the encrypted message's content excluding its first 54 bytes from the encrypted message (p2.bmp). Subsequently, these two portions are combined into a new file named new.bmp.

## Modes

### Original Image

<img src="/images/pic_original.png" alt="Original Image">
<img src="/images/pic_original.bmp" alt="Original Image">

### ECB (Electronic Code Book) 

```bash
openssl enc -aes-128-ecb -e -in pic_original.bmp -out encrypted_image.bmp -K 00112233445566778889aabbccddeeff
head -c 54 pic_original.bmp > header
tail -c +55 encrypted_image.bmp > body
cat header body > new_ecb.bmp
```

#### Output image
<img src="/images/new_ecb.png" alt="ecb-encrypted-image">
<img src="/images/new_ecb.bmp" alt="ecb-encrypted-image">


### CBC (Cipher Block Chaining) 

```bash
openssl enc -aes-128-cbc -e -in pic_original.bmp -out encrypted_image.bmp -K 00112233445566778889aabbccddeeff -iv 0102030405060708
head -c 54 pic_original.bmp > header
tail -c +55 encrypted_image.bmp > body
cat header body > new_cbc.bmp
```

#### Output image
<img src="/images/new_cbc.png" alt="cbc-encrypted-image">
<img src="/images/new_cbc.bmp" alt="cbc-encrypted-image">

### Conclusion:
When using ECB mode for encryption, the resulting image closely resembled the original. Opting for CBC mode, however, proved to be a superior choice for encrypting the image.
