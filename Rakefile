require 'json'

WORD_CLASSES = [
  'adj',
  'adv',
  'auxiliary',
  'conj',
  'definite article',
  'determiner',
  'indefinite article',
  'interjection',
  'modal',
  'n',
  'number',
  'predeterminer',
  'prep',
  'pron',
  'v',
]

PARTS_OF_PHRASES = [
  'another',
  'card',
  'course',
  'cream',
  'morning',
  'night',
  'office',
  'one',
  'other',
  'phone',
  'right',
  'to',
  'way',
]

def parse(str)
  str.split(/\n/).map do |line|
    if /(.*)(S\d),\s*(W\d)\z/ =~ line
      rest = $1.strip
      frequencies = [$2, $3]
    elsif /(.*)([SW]\d)\z/ =~ line
      rest = $1.strip
      frequencies = [$2]
    else
      raise "frequencies not found: #{line.inspect}"
    end

    rest, *word_classes = rest.split(/\s*,\s*/)

    word, *candidates = rest.split(/\s+/)

    while candidates.any?
      if PARTS_OF_PHRASES.include?(candidates.first)
        word += " #{candidates.shift}"
      else
        break
      end
    end

    if candidates.any?
      word_class = candidates.join(' ')

      if WORD_CLASSES.include?(word_class)
        word_classes.unshift(word_class)
      else
        raise "unknown word class: #{word_class.inspect}"
      end
    end

    { word: word, word_classes: word_classes, frequencies: frequencies }
  end
end

task :default => :parse

task :parse do
  entries = parse(File.read('longman-communication-3000.txt'))
  File.write('longman-communication-3000.json', JSON.pretty_generate(entries))
end
