---
title: LeetCode 208.实现Trie(前缀树)
comments: true
toc: true
date: 2019-11-11 21:04:14
categories: 算法题
tags: LeetCode
---

# 题目

实现一个 Trie (前缀树)，包含 insert, search, 和 startsWith 这三个操作。

**示例:**
```java
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // 返回 true
trie.search("app");     // 返回 false
trie.startsWith("app"); // 返回 true
trie.insert("app");   
trie.search("app");     // 返回 true
```

**说明:**

>你可以假设所有的输入都是由小写字母 a-z 构成的。
>保证所有输入均为非空字符串。

# 思路 + 代码

[题解](https://leetcode-cn.com/problems/implement-trie-prefix-tree/solution/shi-xian-trie-qian-zhui-shu-by-leetcode/)

实现一个链表，每一个链表节点存储的是数组，数组包含所有可能的键（这里指26个字符）。

类似于HashMap的entry结构。

```java
class Trie {
    
    private class TrieNode{
        private TrieNode[] links;
        
        private boolean isEnd;
        
        TrieNode(){
            links = new TrieNode[26];
        }
        
        public boolean containsKey(char c){
            return links[c-'a']!=null;
        }
        
        public TrieNode get(char c){
            return links[c-'a'];
        }
        
        public void set(char c, TrieNode node){
            links[c-'a'] = node;
        }
        
        public void setEnd(){
            this.isEnd = true;
        }
        
        public boolean isEnd(){
            return this.isEnd;
        }
    }
    
    private TrieNode root;

    /** Initialize your data structure here. */
    public Trie() {
        root = new TrieNode();
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        TrieNode node = root;
        for(int i=0; i<word.length(); i++){
            char c = word.charAt(i);
            if(!node.containsKey(c)){
                node.set(c, new TrieNode());
            }
            node = node.get(c);
        }
        node.setEnd();
    }
    
    private TrieNode searchPrefix(String word){
        TrieNode node = root;
        for(int i=0; i<word.length(); i++){
            char c = word.charAt(i);
            if(node.containsKey(c))
                node = node.get(c);
            else
                return null;
        }
        return node;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        TrieNode node = searchPrefix(word);
        return node!=null && node.isEnd();
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        TrieNode node = searchPrefix(prefix);
        return node!=null;
    }
    
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```