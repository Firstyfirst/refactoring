# Refactoring - Readability

Code from my PA4-project (Readability) : https://github.com/b6210545602/PA4

## Attribute type and getter method :

In `src/readability/strategy/Readstrategy.java` file :

``` java

public class ReadStrategy {
    /** number of word */
    private int wordNumbers;
    /** number of lines */
    private int lineNumbers;
    /** number of sentences */
    private int sentenceNumbers;
    /** number of syllables */
    private int syllableNumbers;
    /** name of output statement, so tell text come from file or url */
    private String name;

    /**
     * get number of words
     * @return total amount of words 
     */
    public double getWords() {
        return (double) wordNumbers;
    }

    /**
     * get number of lines
     * @return total amount of lines
     */
    public double getLines() {
        return (double) lineNumbers;
    }

    /**
     * get number of sentences
     * @return total amount of sentences
     */
    public double getSentences() {
        return (double) sentenceNumbers;
    }

    /**
     * get numbers of syllables
     * @return total amount of syllables
     */
    public double getSyllables() {
        return (double) syllableNumbers;
    }

    /**
     * get name that determined by file or url
     * @return "file name" or "url path"
     */
    public String getName() {
        return name;
    }
}

```

- Problems :
    - Unnessescary forced type in getter method.
    - Bad attribute naming.
    
- Refactoring :
    - Remove forced type from all getter method.
    - Change the attribute name.
    
``` java

public class ReadStrategy {
    /** number of word */
    private double totalWords; 
    /** name of output statement, so tell text come from file or url */
    private String inputName;
    
    /**
     * get number of words
     * @return total amount of words 
     */
    public double getWords() {
        return this.totalWords;
    }
    
    /**
     * get name that determined by file or url
     * @return "file name" or "url path"
     */
    public String getName() {
        return this.inputName;
    }

```

## countSentence(String line) method :

In `src/readability/strategy/Counter.java` file :

``` java

public int countSentence(String line) {
    int sentencegroups = 0;
    String[] lines = line.split("\\s+");
    for(String word: lines) {
        // the word end with ".", ";", "!", "?"
        if (word.endsWith(".") || word.endsWith(";") || word.endsWith("!") || word.endsWith("?")) {
            String subword = word.substring(0, word.length() - 1);
            if (isNumeric(subword)) sentencegroups--;
            sentencegroups++;
        }
    } // end of the line
    // more code
}

```

- Problem :
    - Naming of method.
    - The suffix checking in if statement is too long.
    
- Refactoring :
    - Change the method name.
    - Replace with contains method and the constant in if statement.
    

``` java

public int numberSentence(String line) {
        int sentenceGroups = 0;
        String[] lines = line.split("\\s+");
        final String SUFFIX = ".;!?";
        for(String word: lines) {
            // the word end with ".", ";", "!", "?"
            //if (word.endsWith(".") || word.endsWith(";") || word.endsWith("!") || word.endsWith("?")) {
            if (SUFFIX.contains(word.substring(word.length()-1))) {
                String subword = word.substring(0, word.length() - 1);
                if (isNumeric(subword)) sentenceGroups--;
                sentenceGroups++;
            }
        } // end of the line
        // more code

```

## countSyllable(String word) method :

In `src/readability/strategy/Counter.java` file :

``` java

public int countSyllable(String word) {
    // some code
}

```

- Problem : Naming of method.

- Refactoring : Change name of method.
    
``` java

public int numberSyllable(String word) {
    // some code
}

```

## countAll(String line) method :

In `src/readability/strategy/ReadStrategy.java` file :

``` java

public void countAll(String line) {
        //count line
        lineNumbers++;
        
        Counter counter = new Counter();
        if (!line.equals("")) {
            String[] wordList = line.split("[.,()_/:!?;]|\\s|\"");
            // count syllable
            for(String word: wordList) {
                int n = counter.countSyllable(word.toLowerCase());
                syllableNumbers += n;
                if (n > 0) {
                    // it is a word
                    wordNumbers++;
                }
            }
        }
        int i = counter.countSentence(line);
        sentenceNumbers += i;
    }

```

- Problems :
    - n valuable is the bad naming.
    - Unnessescary valuable i.
    - "line.equals("")" is not a unsense method.  
  
- Refactoring :
    - Change valuable name "n" to "wordSyllables".
    - Put the "counter.countSentence(line)" in the same line as "sentenceNumbers".
    - Change from "line.equals("")" to "line.isBlank()".
    

``` java

public void countAll(String line) {
        //count line
        totalLines++;
        
        Counter counter = new Counter();
        if (!line.isBlank()) {
            String[] wordList = line.split("[.,()_/:!?;]|\\s|\"");
            // count syllable
            for(String word: wordList) {
                int wordSyllables = counter.numberSyllable(word);
                totalSyllables += wordSyllables;
                //it is a word
                if (wordSyllables > 0) totalWords++;
            }
        }
        totalSentences += counter.numberSentence(line);
    }

```
