# Refactoring - Readability

Code from my PA4-project (Readability) : https://github.com/b6210545602/PA4

## Attribute type and getter method

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

/**
 * count the sentence in a line
 * @param line the line that seperate with \n 
 * @return return number of sentence in a line. 
 * If the line do not have sentence and next line
 * is blank line, count it as sentence
 */
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

    if (sentencegroups == 0) nosentence = true;
    // if there is not a sentence in line 
    // and next line is blank line, count as a sentence
    if (nosentence && line.isBlank()) {
        nosentence = false; 
        sentencegroups++;
    }
    return sentencegroups;
}

```
