import math
from functools import reduce
import pyquil.quil as pq
from pyquil import api
from pyquil.gates import H
from six.moves import range


def qubits_needed(n):
    """
    The number of qubits needed for a die of n faces.
    """
    return int(math.ceil(math.log(n, 2)))

def die_program(n):
    """
    Generate a quantum program to roll a die of n faces.
    """
    prog = pq.Program()
    qubits = qubits_needed(n)
    # Hadamard initialize.
    for q in range(qubits):
        prog.inst(H(q))
    # Measure everything.
    for q in range(qubits):
        prog.measure(q, [q])
    return prog

def process_result(r):
    """
    Convert a list of measurements to a die value.
    """
    return reduce(lambda s, x: 2*s + x, r, 0)

BATCH_SIZE = 10

dice = {}

qvm = api.QVMConnection()

def roll_die(n):
    """
    Roll an n-sided quantum die.
    """
    addresses = list(range(qubits_needed(n)))
    if not n in dice:
        dice[n] = die_program(n)
    die = dice[n]
    # Generate results and do rejection sampling.
    while True:
        results = qvm.run(die, addresses, BATCH_SIZE)
        for r in results:
            x = process_result(r)
            if 0 < x <= n:
                return x

if __name__ == '__main__':
    number_of_sides = int(input("Please enter number of sides: "))
    if True:
        print(roll_die(number_of_sides))
