* **字典树总结**
   * [实现Trie(前缀树)](#实现Trie前缀树) (`medium`)
   * [键值映射](#键值映射) (`medium`)
   * [单词替换](#单词替换) (`medium`)
   * [添加与搜索单词-数据结构设计](#添加与搜索单词-数据结构设计) (`medium`)

# 字典树总结
## 添加与搜索单词-数据结构设计

[LeetCode中文](https://leetcode.cn/problems/design-add-and-search-words-data-structure/solutions/)

**解法**： 前缀树 + 正则匹配

```c++
struct TrieTreeNode {
    std::vector<TrieTreeNode*> sons;
    int num;
    explicit TrieTreeNode(int a) : num(a) {
        sons.resize(26);
        for (int i = 0; i < sons.size(); ++i) {
            sons[i] = nullptr;
        }
    }
};

class TrieTree {
public:
    TrieTree() {
        root = new TrieTreeNode(0);
    }

    void AddStr(const std::string& str) {
        AddStrRecursive(str, root, 0);
    }

    bool Exists(const std::string& str) {
        return ExistsRecursive(str, root, 0);
    }

private:
    TrieTreeNode* root = nullptr;
    void AddStrRecursive(const std::string& str, TrieTreeNode* node, int idx) {
        if (idx == str.size()) {
            ++node->num;
            return;
        }
        int a = str[idx] - 'a';
        if (node->sons[a] == nullptr) {
            node->sons[a] = new TrieTreeNode(0);
        }
        AddStrRecursive(str, node->sons[a], idx + 1);
    }

    bool ExistsRecursive(const std::string& str, const TrieTreeNode* node, int idx) {
        if (node == nullptr) {
            return false;
        }
        if (idx == str.size()) {
            if (node->num > 0) {
                return true;
            } else {
                return false;
            }
        }
        if (str[idx] == '.') {
          for (int i = 0; i < 26; ++i) {
            if (node->sons[i] != nullptr && ExistsRecursive(str, node->sons[i], idx + 1)) {
              return true;
            }
          }
          return false;
        }
        int a = str[idx] - 'a';
        if (node->sons[a] == nullptr) {
            return false;
        }
        return ExistsRecursive(str, node->sons[a], idx + 1);
    }
};

class WordDictionary {
public:
    WordDictionary() {
      tree_ = new TrieTree();
    }
    
    void addWord(string word) {
        tree_->AddStr(word);
    }
    
    bool search(string word) {
        return tree_->Exists(word);
    }

private:
  TrieTree* tree_;
};

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary* obj = new WordDictionary();
 * obj->addWord(word);
 * bool param_2 = obj->search(word);
 */
```

## 实现Trie(前缀树)

[LeetCode中文](https://leetcode-cn.com/problems/implement-trie-prefix-tree)

[LeetCode英文](https://leetcode.com/problems/implement-trie-prefix-tree)

实现一个 Trie (前缀树)，包含 `insert`, `search`, 和 `startsWith` 这三个操作。

**示例**:

```
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // 返回 true
trie.search("app");     // 返回 false
trie.startsWith("app"); // 返回 true
trie.insert("app");   
trie.search("app");     // 返回 true
```

**说明**:

* 你可以假设所有的输入都是由小写字母 a-z 构成的。
* 保证所有输入均为非空字符串。

## 解答

[详见前缀树介绍](https://leetcode-cn.com/explore/learn/card/trie/)

前缀树结点中`tail`表示是否有字符串在该节点处结束，数组`next[26]`表示该节点在前缀树下一层的**26个分支**(如果考虑大小写字母就是**52个分支**)。

设所有字符串的字母数总共是*n*

* 时间复杂度：`insert`为O(*log n*)，`search`为O(*log n*)，`startsWith`为O(*log n*)
* 空间复杂度：O(*n*)

```c++
struct node
{
    int tail;
    node* next[26];
    
    node()
    {
        tail = 0;
        for(int i=0;i<26;i++)
            next[i] = nullptr;
    }
};


class Trie {
public:
    /** Initialize your data structure here. */
    Trie() {
        root = nullptr;
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        if(word.empty())
            return;
        
        if(!root)
            root = new node();
        
        node* p = root;
        for(int i=0;i<word.size();i++)
        {
            int a = word[i] - 'a';
            
            if(!p->next[a])
                p->next[a] = new node();
            p = p->next[a];
        }
        p->tail++;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        if(word.empty())
            return false;
        
        if(!root)
            return false;
        
        node* p = root;
        for(int i=0;i<word.size();i++)
        {
            int a = word[i]-'a';
            if(!p->next[a])
                return false;
            p = p->next[a];
        }
        
        if(p->tail > 0)
            return true;
        else
            return false;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        if(prefix.empty())
            return false;
        
        if(!root)
            return false;
        
        node* p = root;
        for(int i=0;i<prefix.size();i++)
        {
            int a = prefix[i] - 'a';
            if(!p->next[a])
                return false;
            p = p->next[a];
        }
        
        return true;
    }
    
private:
    node* root;
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * bool param_2 = obj.search(word);
 * bool param_3 = obj.startsWith(prefix);
 */
```

## 键值映射

[LeetCode中文](https://leetcode-cn.com/problems/map-sum-pairs)

[LeetCode英文](https://leetcode.com/problems/map-sum-pairs)

实现一个 MapSum 类里的两个方法，`insert` 和 `sum`。

对于方法 `insert`，你将得到一对（字符串，整数）的键值对。字符串表示键，整数表示值。如果键已经存在，那么原来的键值对将被替代成新的键值对。

对于方法 `sum`，你将得到一个表示前缀的字符串，你需要返回所有以该前缀开头的键的值的总和。

**示例 1**:

```
输入: insert("apple", 3), 输出: Null
输入: sum("ap"), 输出: 3
输入: insert("app", 2), 输出: Null
输入: sum("ap"), 输出: 5
```

## 解答

基于插入的键值对的`key`(**类型为字符串**)建立前缀树，在前缀树结点中加入元素`val`，表示以**在这个节点结束的字符串**为`key`对应的`value`。

* 调用`insert`时，实现过程和 [实现Trie](#实现Trie前缀树) 的 `insert` 方法类似；
* 调用`sum`时，得到所有以`prefix`为前缀的字符串作为`key`对应的`value`，将它们加起来得出结果。(利用树的性质，可以**递归**地处理这个过程，详见代码)



设所有`key`对应字符串的字母数总共是*n*

* 时间复杂度：`insert`为O(*log n*)，`sum`为O(*log n*)
* 空间复杂度：O(*n*)

```c++
struct node
{
    bool tail;
    int val;
    node* next[26];
    node()
    {
        tail = false;
        val = 0;
        for(int i=0;i<26;i++)
            next[i] = nullptr;
    }
};

void recursion(node* p,int& sum)
{
    if(p->tail)
    {
        sum += p->val;
    }
    
    for(int i=0;i<26;i++)
    {
        if(p->next[i])
            recursion(p->next[i],sum);
    }
}

class MapSum {
public:
    /** Initialize your data structure here. */
    MapSum() {
        root = nullptr;
    }
    
    void insert(string key, int val) {
        if(key.empty())
            return;
        
        if(!root)
            root = new node();
        
        node* p = root;
        for(auto a : key)
        {
            int b = a - 'a';
            
            if(!p->next[b])
                p->next[b] = new node();
            
            p = p->next[b];
        }
        
        p->tail = true;
        p->val = val;
    }
    
    int sum(string prefix) {
        if(prefix.empty() || !root)
            return 0;
        
        node* p = root;
        for(auto a : prefix)
        {
            int b = a - 'a';
            if(!p->next[b])
                return 0;
            
            p = p->next[b];
        }
        
        int sum = 0;
        recursion(p,sum);
        
        return sum;
    }
    
private:
    node* root;
};

/**
 * Your MapSum object will be instantiated and called as such:
 * MapSum obj = new MapSum();
 * obj.insert(key,val);
 * int param_2 = obj.sum(prefix);
 */
```



## 单词替换

[LeetCode中文](https://leetcode-cn.com/problems/replace-words)

[LeetCode英文](https://leetcode.com/problems/replace-words)

在英语中，我们有一个叫做 `词根`(root)的概念，它可以跟着其他一些词组成另一个较长的单词——我们称这个词为 `继承词`(successor)。例如，词根`an`，跟随着单词 `other`(其他)，可以形成新的单词 `another`(另一个)。

现在，给定一个由许多词根组成的词典和一个句子。你需要将句子中的所有`继承词`用`词根`替换掉。如果`继承词`有许多可以形成它的`词根`，则用最短的词根替换它。

你需要输出替换之后的句子。

**示例 1**:

```
输入: dict(词典) = ["cat", "bat", "rat"]
sentence(句子) = "the cattle was rattled by the battery"
输出: "the cat was rat by the bat"
```

**注意**:

* 输入只包含小写字母。
* 1 <= 字典单词数 <=1000
* 1 <=  句中词语数 <= 1000
* 1 <= 词根长度 <= 100
* 1 <= 句中词语长度 <= 1000

## 解答

基于所有的`词根`建立前缀树，遍历句子`sentence`中的每一个单词`word`，逐个在前缀树中查找是否存在有`word`的前缀：如果存在，则将`word`替换成找到的最短前缀；否则，忽略。



设`sentence`中的单词数为*m*，词典中的`词根`总字母数为*n*

* 时间复杂度：O(*m log n*)
* 空间复杂度：O(*n*)

```c++
struct node
{
    bool tail;
    node* next[52];
    node()
    {
        tail=false;
        for(int i=0;i<52;i++)
            next[i] = nullptr;
    }
    
};

class Trie
{
public:
    Trie()
    {
        root = nullptr;
    }
    
    void insert(string word)
    {
        if(word.empty())
            return;
        
        if(!root)
            root = new node();
        
        node* p = root;
        for(int i=0;i<word.size();i++)
        {
            int a = word[i]-'a';
            
            if(!p->next[a])
                p->next[a] = new node();
            p=p->next[a];
        }
        
        p->tail = true;
    }
    
    void split(string& word)
    {
        int cnt = 0;
        
        node* p = root;
        string res;
        for(auto s : word)
        {
            int a = s-'a';
            
            if(p && p->tail)
                break;
            if(p->next[a])
                p = p->next[a];
            else
                break;
            
            cnt++;
        }
        
        if(p && p->tail)
            word = word.substr(0,cnt);
    }
    
private:
    node* root;
    
};


class Solution {
public:
    string replaceWords(vector<string>& dict, string sentence) {
        Trie tr;
        for(auto a : dict)
            tr.insert(a);
        
        vector<string> res;
        string tmp("");
        for(auto s : sentence)
        {
            if(s != ' ')
                tmp += s;
            else
            {
                res.push_back(tmp);
                tmp.clear();
            }
        }
        
        res.push_back(tmp);
        
        string out;
        for(auto w : res)
        {
            tr.split(w);
            out += w + ' ';
        }
        
        out.pop_back();
        
        return out;
    }
};
```
