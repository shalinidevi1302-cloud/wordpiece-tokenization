from collections import Counter

with open("dataset/rawdata.txt", "r") as file:
    words = [line.strip().lower() for line in file if line.strip()]

word_frequency = Counter(words)

print("========== WORD FREQUENCIES ==========")

for word, frequency in word_frequency.items():
    print(word, ":", frequency)

splits = {}

for word in word_frequency:
    tokens = [word[0]]

    for character in word[1:]:
        tokens.append("##" + character)

    splits[word] = tokens

print("\n========== INITIAL WORD SPLITS ==========")

for word, tokens in splits.items():
    print(word, "->", tokens)

vocabulary = set()

for tokens in splits.values():
    vocabulary.update(tokens)

print("\n========== INITIAL VOCABULARY ==========")

for token in sorted(vocabulary):
    print(token)


def get_token_frequency():
    frequencies = Counter()

    for word, tokens in splits.items():
        for token in tokens:
            frequencies[token] += word_frequency[word]

    return frequencies


def get_pair_frequency():
    frequencies = Counter()

    for word, tokens in splits.items():
        frequency = word_frequency[word]

        for i in range(len(tokens) - 1):
            pair = (tokens[i], tokens[i + 1])
            frequencies[pair] += frequency

    return frequencies


def calculate_scores(pair_frequency, token_frequency):
    scores = {}

    for pair, frequency in pair_frequency.items():
        first = pair[0]
        second = pair[1]

        scores[pair] = frequency / (
            token_frequency[first] * token_frequency[second]
        )

    return scores


def merge_pair(best_pair):
    new_token = best_pair[0] + best_pair[1].replace("##", "")

    for word in splits:
        tokens = splits[word]
        new_tokens = []
        i = 0

        while i < len(tokens):
            if (
                i < len(tokens) - 1
                and tokens[i] == best_pair[0]
                and tokens[i + 1] == best_pair[1]
            ):
                new_tokens.append(new_token)
                i += 2
            else:
                new_tokens.append(tokens[i])
                i += 1

        splits[word] = new_tokens

    return new_token


initial_vocabulary_size = len(vocabulary)
target_vocabulary_size = initial_vocabulary_size + 6

merge_rules = []

print("\n========== WORDPIECE TRAINING ==========")

while len(vocabulary) < target_vocabulary_size:

    pair_frequency = get_pair_frequency()

    if not pair_frequency:
        break

    token_frequency = get_token_frequency()

    scores = calculate_scores(
        pair_frequency,
        token_frequency
    )

    best_pair = max(scores, key=scores.get)
    best_score = scores[best_pair]

    new_token = merge_pair(best_pair)

    vocabulary.add(new_token)

    merge_rules.append((best_pair, new_token))

    print("\nMerge", len(merge_rules))
    print("Best Pair :", best_pair)
    print("Pair Frequency :", pair_frequency[best_pair])
    print("Score :", round(best_score, 6))
    print("New Token :", new_token)


print("\n========== FINAL WORD SPLITS ==========")

for word, tokens in splits.items():
    print(word, "->", tokens)


vocabulary.add("[UNK]")

print("\n========== FINAL VOCABULARY ==========")

for token in sorted(vocabulary):
    print(token)

print("\nVocabulary Size :", len(vocabulary))


def tokenize_word(word):
    tokens = []

    while word:

        found = False

        for length in range(len(word), 0, -1):

            part = word[:length]

            if tokens:
                part = "##" + part

            if part in vocabulary:

                tokens.append(part)
                word = word[length:]
                found = True
                break

        if not found:
            return ["[UNK]"]

    return tokens


test_word = "player"

tokens = tokenize_word(test_word)

print("\n========== TOKENIZATION ==========")

print("Input Word :", test_word)
print("Tokens :", tokens)


token_to_id = {}

for index, token in enumerate(sorted(vocabulary)):
    token_to_id[token] = index

token_ids = [token_to_id[token] for token in tokens]

print("\n========== TOKEN IDs ==========")

for token, token_id in token_to_id.items():
    print(token, ":", token_id)

print("\nInput Tokens :", tokens)
print("Token IDs :", token_ids)

print("\n========== PROGRAM COMPLETED ==========")