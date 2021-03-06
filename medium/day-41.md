# 208.实现 Trie

https://leetcode-cn.com/problems/implement-trie-prefix-tree

## 题目描述

```
实现一个 Trie (前缀树)，包含 insert, search, 和 startsWith 这三个操作。

示例:

Trie trie = new Trie();

trie.insert("apple");
trie.search("apple"); // 返回 true
trie.search("app"); // 返回 false
trie.startsWith("app"); // 返回 true
trie.insert("app");
trie.search("app"); // 返回 true
说明:

你可以假设所有的输入都是由小写字母 a-z 构成的。
保证所有输入均为非空字符串。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/implement-trie-prefix-tree
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## 思路

熟悉一下 Trie 的概念就可以了。

https://medium.com/basecs/trying-to-understand-tries-3ec6bede0014

## 输入输出

Nodejs

```js
const __main__ = function () {
    const readline = require('readline')
    const rl = readline.createInterface({
        input: process.stdin,
        output: process.stdout,
    })

    console.log('******输入******')
    rl.prompt()
    const lines = []
    rl.on('line', line => lines.push(line))

    rl.on('close', () => {
        console.log('\n******输出******')

        const test = (operations, params) => {
            if (operations[0] === 'Trie') {
                const trie = new Trie()
                const output = [null]
                for (let i = 1; i < operations.length; i++) {
                    const res = trie[operations[i]](...params[i])
                    output.push(res === void 0 ? null : res)
                }
                console.log(output)
            } else {
                console.log(Array(operations.length).fill(null))
            }
        }

        while (lines.length >= 2) {
            const params = lines.splice(0, 2)
            test(...params.map(el => JSON.parse(el)))
        }
    })
}
```

## 复杂度分析

-   时间复杂度：$O(L)$，L 是字符串长度， `insert` `search` `startsWith` 操作都是。
-   空间复杂度：$O(M^{L})$，L 是字符串长度，M 是字符集中字符个数，如本题中 M 就是 26。

## 代码

TypeScript Code

```ts
class TrieNode {
    value: string
    children: Array<TrieNode | null>

    constructor(value) {
        this.value = value
        this.children = Array(26)
    }
}

class Trie {
    private root: TrieNode

    constructor() {
        this.root = this._getTrieNode('')
    }

    private _getTrieNode(value: string): TrieNode {
        return new TrieNode(value)
    }

    private _char2Index(char: string): number {
        return char.toLowerCase().charCodeAt(0) - 97
    }

    insert(word: string): void {
        let crawl: TrieNode = this.root
        for (let char of word) {
            const index: number = this._char2Index(char)
            if (!crawl.children[index]) {
                crawl.children[index] = this._getTrieNode('')
            }
            crawl = crawl.children[index]
        }
        crawl.value = word
    }

    search(word: string): boolean {
        let crawl: TrieNode = this.root
        for (let char of word) {
            const index: number = this._char2Index(char)
            if (!crawl.children[index]) return false
            crawl = crawl.children[index]
        }
        return crawl.value === word
    }

    startsWith(prefix: string): boolean {
        let crawl: TrieNode = this.root
        for (let char of prefix) {
            const index: number = this._char2Index(char)
            if (!crawl.children[index]) return false
            crawl = crawl.children[index]
        }
        return true
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * var obj = new Trie()
 * obj.insert(word)
 * var param_2 = obj.search(word)
 * var param_3 = obj.startsWith(prefix)
 */
```

JavaScript Code

```js
class TrieNode {
    constructor(val) {
        this.value = val
        this.pointers = Array(26)
    }
}

class Trie {
    constructor() {
        this.root = this._getTrieNode('')
    }

    /**
     * @param {string} val
     */
    _getTrieNode(val) {
        return new TrieNode(val)
    }

    /**
     * @param {string} char
     * @returns {number}
     */
    _char2Index(char) {
        return char.toLowerCase().charCodeAt(0) - 97
    }

    /**
     * Inserts a word into the trie.
     * @param {string} word
     * @return {void}
     */
    insert(word) {
        let crawl = this.root
        for (let char of word) {
            const index = this._char2Index(char)
            if (!crawl.pointers[index]) {
                crawl.pointers[index] = this._getTrieNode('')
            }
            crawl = crawl.pointers[index]
        }
        // Store the word in the last TrieNode as an end mark.
        crawl.value = word
    }

    /**
     * Returns if the word is in the trie.
     * @param {string} word
     * @return {boolean}
     */
    search(word) {
        let crawl = this.root
        for (let char of word) {
            const index = this._char2Index(char)
            if (!crawl.pointers[index]) return false

            crawl = crawl.pointers[index]
        }
        // If it has a stored value, it is the last TrieNode, i.e., the desired word is found.
        // Otherwise, the word doesn't exist in Trie.
        return !!crawl.value
    }

    /**
     * Returns if there is any word in the trie that starts with the given prefix.
     * @param {string} prefix
     * @return {boolean}
     */
    startsWith(prefix) {
        let crawl = this.root
        for (let char of prefix) {
            const index = this._char2Index(char)
            if (!crawl.pointers[index]) return false

            crawl = crawl.pointers[index]
        }
        return true
    }
}
```

**官方题解**

https://github.com/leetcode-pp/91alg-1/issues/68#issuecomment-657077878
