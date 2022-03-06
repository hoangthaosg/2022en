'use strict'

// alert('hello algorithm');

//1. get product of all other elements
function productOthers(arr) {
    let prefix = [];
    let accumulator = 1;
    for (let i = 0; i< arr.length; i++) {
        if (i > 0) {
            accumulator *= arr[i-1];   
        } 
        prefix.push(accumulator);
        
        
    }
    console.log('prefix', prefix);
    accumulator = 1;
    let suffix = [];
    for (let i = arr.length - 1; i >= 0; i--) {
        if (i < arr.length - 1) {
            accumulator *= arr[i+1];
        } 
        suffix.push(accumulator);
    }
    console.log('suffix', suffix);
    let rs = [];
    for (let i = 0; i < arr.length; i++) {
        rs.push(prefix[i] * suffix[arr.length - i - 1]);
    }
    return rs;
}

//prefix 1, 1, 2, 6, 24
//suffix 120, 60, 20, 5, 1 (revert)
//console.log('productOthers([1,2,3,4,5]), expected [120, 60, 40, 30, 24]', productOthers([1,2,3,4,5]));
//console.log('productOthers([3,2,1]) expected [2, 3, 6]', productOthers([3,2,1]))

// merge sort
function mergeSort(arr, left, right) {
    while (left < right) {
        let mid = Math.floor((right - left) / 2);
        mergeSort(arr, left, left + mid);
        mergeSort(arr, left + mid, right);
        //merge(left)
    }
    return arr;
}

// console.log(mergeSort([1,2,3,4,5], 0, 4));

// 2. Find smallest window to be sorted
// naive 0(n) = nlogn
function smallestWindowToSorted(arr) {
    console.log('arr', arr);
    let left = -1, right = arr.length-1;
    let sorted = [...arr].sort(); // clone new array
    console.log('sorted', sorted);
    for (let i = 0; i < arr.length; i++) {
        if (arr[i] != sorted[i] && left == -1) {
            left = i;
        }
        if (arr[i] != sorted[i]) {
            right = i;
        }
    }
    return [left, right];
}

// console.log(smallestWindowToSorted([3,7,5,6,9]))

class TreeNode {
    constructor(value) {
        this.value = value;
        this.left = null;
        this.right = null;
    }
}

let node = new TreeNode(12);
node.left = new TreeNode(13);
node.right = new TreeNode(14);
node.left.left = new TreeNode(15);
node.left.right = new TreeNode(16);
node.right.left = new TreeNode(17);
node.right.right = new TreeNode(18);

function bfs(node) {
    let rs = [];
    let queue = [node];
    while (queue.length > 0) {
        let size = queue.length;
        let currLevel = [];
        for (let i = 0; i < size; i++) {
            let currNode = queue.shift();
            currLevel.push(currNode);
            if (currNode.left) queue.push(currNode.left);
            if (currNode.right) queue.push(currNode.right);
        }
        rs.push(currLevel);
    }
    return rs;
}

//console.log(bfs(node));

function bfs_reverse(node) {
    let rs = [];
    let queue = [node];
    while (queue.length > 0) {
        let size = queue.length;
        let currLevel = [];
        for (let i = 0; i < size; i++) {
            let currNode = queue.shift();
            currLevel.push(currNode);
            if (currNode.left) queue.push(currNode.left);
            if (currNode.right) queue.push(currNode.right);
        }
        rs.unshift(currLevel);
    }
    return rs;
}

//console.log(bfs_reverse(node));

function zigzag_bfs(node) {
    let rs = [];
    let queue = [node];
    let reverse = false;
    while (queue.length > 0) {
        let size = queue.length;
        let currLevel = [];
        for (let i = 0; i < size; i++) {
            let currNode = queue.shift();
            if (reverse) {
                currLevel.unshift(currNode);
            } else {
                currLevel.push(currNode);
            }
            if (currNode.left) queue.push(currNode.left);
            if (currNode.right) queue.push(currNode.right);
        }
        rs.push(currLevel);
        reverse = !reverse
    }
    return rs;
}

//console.log(zigzag_bfs(node));

function level_average_bfs(node) {
    let rs = [];
    let queue = [node];
    while (queue.length > 0) {
        let size = queue.length;
        let sum = 0;
        for (let i = 0; i < size; i++) {
            let currNode = queue.shift();
            sum += currNode.value;
            if (currNode.left) queue.push(currNode.left);
            if (currNode.right) queue.push(currNode.right);
        }
        rs.push(sum/size);
    }
    return rs;
}

//console.log(level_average_bfs(node));

function minimum_depth_bfs(node) {
    let queue = [node];
    let level = 0;
    while (queue.length > 0) {
        level++;
        let size = queue.length;
        for (let i = 0; i < size; i++) {
            let currNode = queue.shift();
            // is leaf node
            if (currNode.left == null && currNode.right == null) {
                return level;
            } 
            if (currNode.left) queue.push(currNode.left);
            if (currNode.right) queue.push(currNode.right);
        }
    }
    return level;
}

/*
(function() {
    let node = new TreeNode(1);
    node.left = new TreeNode(2);
    node.right = new TreeNode(3);
    node.left.left = new TreeNode(4);
    node.left.right = new TreeNode(5);
    console.log(minimum_depth_bfs(node));
})()
*/

function find_successor_bfs(node, val) {
    let queue = [node];
    while (queue.length > 0) {
        let currNode = queue.shift();
        if (currNode.left) queue.push(currNode.left);
        if (currNode.right) queue.push(currNode.right);
        if (currNode.value == val){ 
            break;
        }
    }
    if (queue.length > 0) {
        return queue.shift();
    }
    return null;
}

/*
(function() {
    let node = new TreeNode(1);
    node.left = new TreeNode(2);
    node.right = new TreeNode(3);
    node.left.left = new TreeNode(4);
    node.left.right = new TreeNode(5);
    console.log(find_successor_bfs(node, 3).value);

    node = new TreeNode(12);
    node.left = new TreeNode(7);
    node.right = new TreeNode(1);
    node.left.right = new TreeNode(9);
    node.right.left = new TreeNode(10);
    node.right.right = new TreeNode(5);
    console.log(find_successor_bfs(node, 9).value);
})()
*/

class TreeNode2 {
    constructor(value) {
        this.value = value;
        this.left = null;
        this.right = null;
        this.next = null;
    }

    print_level_order() {
        console.log('Level order traversal using next pointer')
        let nextLvlRoot = this;
        while (nextLvlRoot != null) {
            let curr = nextLvlRoot;
            // after begin lvlRoot then reset null
            nextLvlRoot = null;
            while (curr != null) {
                console.log(curr.value);
                // find nextLvlRoot because curr is currLevelRoot
                if (nextLvlRoot == null) { // curr is level root
                    if (curr.left != null) nextLvlRoot = curr.left;
                    else if (curr.right != null) nextLvlRoot = curr.right;
                }
                curr = curr.next;
            }
        }
    }

    print_level_order_complete() {
        console.log('Level order traversal using next pointer complete')
        let curr = this;
        while (curr != null) {
            console.log(curr.value);
            curr = curr.next;
        }
    }
}

function connect_level_sibling_bfs(node) {
    let queue = [node];
    while (queue.length > 0) {
        let size = queue.length;
        let prev = null;
        for (let i = 0; i < size; i++) {
            let currNode = queue.shift();
            if (prev != null) prev.next = currNode;
            if (currNode.left) queue.push(currNode.left);
            if (currNode.right) queue.push(currNode.right);
            prev = currNode;
        }
    }
}

// (function() {
//     let node = new TreeNode2(12);
//     node.left = new TreeNode2(7);
//     node.right = new TreeNode2(1);
//     node.left.right = new TreeNode2(9);
//     node.right.left = new TreeNode2(10);
//     node.right.right = new TreeNode2(5);
//     connect_level_sibling_bfs(node);
//     //console.log(node);
//     node.print_level_order();
// })()

function connect_sibling_bfs(node) {
    let queue = [node];
    let prev = null;
    while (queue.length > 0) {
        let currNode = queue.shift();
        if (prev != null) prev.next = currNode;
        if (currNode.left) queue.push(currNode.left);
        if (currNode.right) queue.push(currNode.right);
        prev = currNode;
    }
}

// (function() {
//     let node = new TreeNode2(12);
//     node.left = new TreeNode2(7);
//     node.right = new TreeNode2(1);
//     node.left.right = new TreeNode2(9);
//     node.right.left = new TreeNode2(10);
//     node.right.right = new TreeNode2(5);
//     connect_sibling_bfs(node);
//     console.log(node);
//     node.print_level_order_complete();
// })()

function tree_right_view(node) {
    let rs = [];
    let queue = [node];
    while (queue.length > 0) {
        let size = queue.length;
        for (let i = 0; i < size; i++) {
            let currNode = queue.shift();
            if (currNode.left) queue.push(currNode.left);
            if (currNode.right) queue.push(currNode.right);
            if (i == size - 1) rs.push(currNode);
        }
    }
    return rs;
}

(function() {
    let node = new TreeNode(12);
    node.left = new TreeNode(7);
    node.right = new TreeNode(1);
    node.left.right = new TreeNode(9);
    node.right.left = new TreeNode(10);
    node.right.right = new TreeNode(5);
    node.left.right.right = new TreeNode(3);
    console.log(tree_right_view(node));
})()







