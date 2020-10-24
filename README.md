# Refactoring - Readability

Code from my PA4-project (Readability) : https://github.com/b6210545602/PA4

## Refactoring

In `strategy/Readstrategy.java` file :

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
