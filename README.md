# EXP-7-UNIFICATION-RESOLUTION
def unify(x, y, subs=None):
    if subs is None:
        subs = {}
    if x == y:
        return subs
    if isinstance(x, str) and x.islower():
        return unify_var(x, y, subs)
    if isinstance(y, str) and y.islower():
        return unify_var(y, x, subs)
    if isinstance(x, tuple) and isinstance(y, tuple) and len(x) == len(y):
        for xi, yi in zip(x, y):
            subs = unify(xi, yi, subs)
            if subs is None:
                return None
        return subs
    return None
def unify_var(var, x, subs):
    if var in subs:
        return unify(subs[var], x, subs)
    elif isinstance(x, str) and x in subs:
        return unify(var, subs[x], subs)
    elif occurs_check(var, x, subs):
        return None
    else:
        subs[var] = x
        return subs

def occurs_check(var, x, subs):
    if var == x:
        return True
    if isinstance(x, tuple):
        return any(occurs_check(var, xi, subs) for xi in x)
    return False

# Example usage
x = ('parent', 'X', 'Y')
y = ('parent', 'john', 'Y')

result = unify(x, y)
print("Unification result:", result)

