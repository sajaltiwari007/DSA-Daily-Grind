## https://leetcode.com/problems/parsing-a-boolean-expression/description/
Basic Stack implementation Question. Good Problem for practicing or Revising Stacks.

## Java Solution


      class Solution {
    public boolean parseBoolExpr(String s) {
    Stack<Character> symbol = new Stack<>();
        Stack<Character> var = new Stack<>();

        int ind = 0;
        while(ind<s.length()){

            char c = s.charAt(ind);
            if(c==',') {
                ind++;
                continue;
            }
            if(c=='!' || c=='|' || c=='&'){
                symbol.push(c);
            }
            else if(c=='(' || c=='f' || c=='t') var.push(c);
           
            else{
                System.out.println(ind);
                if(symbol.peek()=='|'){
                    symbol.pop();
                    boolean flag = false;
                    while(var.peek()!='('){
                           if(var.peek()=='t') flag = true;
                           var.pop();
                    }
                    var.pop();
                    if(flag) var.push('t');
                    else var.push('f');
                }
                else if(symbol.peek()=='&'){
                    symbol.pop();
                    boolean flag = true;
                    while(var.peek()!='('){
                           if(var.peek()=='f') flag = false;
                           var.pop();
                    }
                    var.pop();
                    if(flag) var.push('t');
                    else var.push('f');
                }
                else{
                    symbol.pop();
                    if(var.peek()=='f'){
                        var.pop();
                        var.pop();
                        var.push('t');
                    }
                    else{
                        var.pop();
                        var.pop();
                        var.push('f');
                    }
                }
            }
            ind++;
        }

        return var.peek()=='t'?true:false;
        
    }}
