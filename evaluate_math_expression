def calc(expression):
    expression = expression.replace(' ', '')
    # quick shortcut in case the expression (called recursively) is just a number
    if not any(x in expression for x in '()*/+-'):
        return float(expression)
    # solving the parenthesis first, with a recursive call
    while "(" in expression:
        loc_b = expression.find('(')
        op_count, cl_count, i = 0, 0, 0
        for c in expression[loc_b:]:
            op_count += 1 if c == '(' else 0
            cl_count += 1 if c == ')' else 0
            i += 1
            if cl_count == op_count:
                break
        loc_b2 = loc_b + i -1
        expression = expression[:loc_b]+str(calc(expression[loc_b+1: loc_b2]))+expression[loc_b2+1:]
    # reformating the string to remove any double operator containing a '-'...
    replace_lst = ['--', '*-', '/-']
    replace_lst2 = ['+', '*_', '/_']
    replace_dict = {k:v for k, v in zip(replace_lst, replace_lst2)}
    while any(forbidden_string in expression for forbidden_string in replace_lst):
        for forbidden_string in replace_lst:
            expression = expression.replace(forbidden_string, replace_dict[forbidden_string])
    # ...and removing any double iterator with a '+', as well as any potential '+' at the beginning of the string
    for i in range(len(expression)):
        if expression[i] == '+' and expression[i+1] in '+-*/' or expression[i] == '+' and expression[i-1] in '+-*/':
            expression = expression[:i]+'$'+expression[i+1:]
    expression = expression.replace('$', '')
    if expression[0] == '+':
        expression = expression[1:]
    # turning the expression into a list of numbers and operators
    for operator in '*/+-':
        expression = expression[0]+expression[1:].replace(operator, f' {operator} ')
    expression_lst = expression.split(' ')
    expression_lst = [x.replace('_', '-') for x in expression_lst]
    # taking care of the '*' and '/' operations...
    expression_lst = simple_op(expression_lst, '*', '/')
    # taking care of the '+' and '-' operations...
    expression_lst = simple_op(expression_lst, '+', '-')
    return float(*expression_lst)


def simple_op(lst, op1, op2):
    operation_dict = {'*': lambda x, y : x*y, '/': lambda x, y : x/y, '+': lambda x, y : x+y, '-': lambda x, y : x-y}
    while any(operator in lst for operator in (op1, op2)):
        loc = min(lst.index(op1) if op1 in lst else float('inf'), lst.index(op2) if op2 in lst else float('inf'))
        op = operation_dict[lst[loc]]
        numbers = lst[loc - 1: loc + 2]
        lst[loc] = op(float(numbers[0]), float(numbers[2]))
        del lst[loc + 1]
        del lst[loc - 1]
    return lst
