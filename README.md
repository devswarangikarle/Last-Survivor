# Last-Survivor

In a different world, there live two violent groups of people, say Group X and Group Y. One day, a fight starts between the two groups, and they start killing people of other groups. As soon as two people from different groups encounter each other, one kills the other. Any person in group X kills any other person in group Y with probability Pxy​, and any person in group Y kills any other person in group Y with probability 1 - Pxy​. Assume at once only one fight takes place between two people.
You have been given Pxy​ as a fraction x/y​ and the initial population of group X and group Y as Nx​ and Ny​.
Calculate the probabilities as a fraction that

at the end of the fight, only people of group X survive and
at the end of the fight, only people of group Y survive.
Input
The first line contains a single integer T, the number of test cases. The description of the test cases follows.
The first line contains two integers x and y, where x/y equals Pxy, the probability that any person of group X kills any other person of group Y in a fight.
The second line of each test case contains two integers Nx and Ny, the initial populations of group X and group Y respectively.

Constraints
1 ≤ T ≤ 5
1 ≤ Nx ≤ 103
1 ≤ Ny ≤ 103
0 ≤ x ≤ y
1 ≤ y ≤ 104


MOD = int(1e9 + 7)

def mod_inverse(a, m):
    return power(a, m - 2, m)

def power(base, exp, mod):
    result = 1
    while exp > 0:
        if exp & 1:
            result = (result * base) % mod
        base = (base * base) % mod
        exp >>= 1
    return result

def main():
    import sys
    input = sys.stdin.read
    data = input().split()

    T = int(data[0])  # Number of test cases
    index = 1
    results = []

    for _ in range(T):
        x = int(data[index])
        y = int(data[index + 1])
        Nx = int(data[index + 2])
        Ny = int(data[index + 3])
        index += 4

        Y_inv = mod_inverse(y, MOD)
        
        dp = [[0] * (Ny + 1) for _ in range(Nx + 1)]

        for i in range(Nx + 1):
            dp[i][0] = 1

        for j in range(1, Ny + 1):
            dp[0][j] = 0

        for i in range(1, Nx + 1):
            for j in range(1, Ny + 1):
                dp[i][j] = (x * dp[i][j - 1] % MOD + (y - x) * dp[i - 1][j] % MOD) % MOD
                dp[i][j] = (dp[i][j] * Y_inv) % MOD

        px_survive = dp[Nx][Ny]
        py_survive = (1 - px_survive + MOD) % MOD

        results.append(f"{px_survive} {py_survive}")

    print("\n".join(results))

if __name__ == "__main__":
    main()

    
