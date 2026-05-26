import math
import re
from typing import Union, List, Tuple

class Calculator:
    """A comprehensive calculator supporting basic and scientific operations."""
    
    def __init__(self):
        """Initialize the calculator with history tracking."""
        self.history: List[Tuple[str, Union[int, float]]] = []
    
    # Basic Operations
    def add(self, a: Union[int, float], b: Union[int, float]) -> Union[int, float]:
        """Add two numbers."""
        result = a + b
        self._record_operation(f"{a} + {b}", result)
        return result
    
    def subtract(self, a: Union[int, float], b: Union[int, float]) -> Union[int, float]:
        """Subtract two numbers."""
        result = a - b
        self._record_operation(f"{a} - {b}", result)
        return result
    
    def multiply(self, a: Union[int, float], b: Union[int, float]) -> Union[int, float]:
        """Multiply two numbers."""
        result = a * b
        self._record_operation(f"{a} * {b}", result)
        return result
    
    def divide(self, a: Union[int, float], b: Union[int, float]) -> Union[int, float]:
        """Divide two numbers with error handling."""
        if b == 0:
            raise ValueError("Cannot divide by zero!")
        result = a / b
        self._record_operation(f"{a} / {b}", result)
        return result
    
    def modulus(self, a: Union[int, float], b: Union[int, float]) -> Union[int, float]:
        """Get the remainder of division."""
        if b == 0:
            raise ValueError("Cannot perform modulus with zero!")
        result = a % b
        self._record_operation(f"{a} % {b}", result)
        return result
    
    def power(self, base: Union[int, float], exponent: Union[int, float]) -> Union[int, float]:
        """Raise base to the power of exponent."""
        result = base ** exponent
        self._record_operation(f"{base} ** {exponent}", result)
        return result
    
    # Scientific Operations
    def sqrt(self, n: Union[int, float]) -> float:
        """Calculate square root."""
        if n < 0:
            raise ValueError("Cannot calculate square root of negative number!")
        result = math.sqrt(n)
        self._record_operation(f"sqrt({n})", result)
        return result
    
    def sin(self, angle_deg: Union[int, float]) -> float:
        """Calculate sine (input in degrees)."""
        angle_rad = math.radians(angle_deg)
        result = math.sin(angle_rad)
        self._record_operation(f"sin({angle_deg}°)", result)
        return result
    
    def cos(self, angle_deg: Union[int, float]) -> float:
        """Calculate cosine (input in degrees)."""
        angle_rad = math.radians(angle_deg)
        result = math.cos(angle_rad)
        self._record_operation(f"cos({angle_deg}°)", result)
        return result
    
    def tan(self, angle_deg: Union[int, float]) -> float:
        """Calculate tangent (input in degrees)."""
        angle_rad = math.radians(angle_deg)
        result = math.tan(angle_rad)
        self._record_operation(f"tan({angle_deg}°)", result)
        return result
    
    def log(self, n: Union[int, float], base: int = 10) -> float:
        """Calculate logarithm (default base 10)."""
        if n <= 0:
            raise ValueError("Logarithm undefined for numbers <= 0!")
        result = math.log(n, base)
        self._record_operation(f"log_{base}({n})", result)
        return result
    
    def ln(self, n: Union[int, float]) -> float:
        """Calculate natural logarithm (base e)."""
        if n <= 0:
            raise ValueError("Natural logarithm undefined for numbers <= 0!")
        result = math.log(n)
        self._record_operation(f"ln({n})", result)
        return result
    
    def factorial(self, n: int) -> int:
        """Calculate factorial of n."""
        if n < 0:
            raise ValueError("Factorial undefined for negative numbers!")
        if not isinstance(n, int):
            raise ValueError("Factorial requires an integer input!")
        result = math.factorial(n)
        self._record_operation(f"{n}!", result)
        return result
    
    def absolute(self, n: Union[int, float]) -> Union[int, float]:
        """Get absolute value."""
        result = abs(n)
        self._record_operation(f"abs({n})", result)
        return result
    
    # Utility Methods
    def evaluate_expression(self, expression: str) -> Union[int, float]:
        """
        Evaluate a mathematical expression safely.
        Supports: +, -, *, /, %, **, sqrt, sin, cos, tan, log, ln, abs, factorial
        """
        try:
            # Replace mathematical functions with safe equivalents
            safe_expression = expression.replace('^', '**')
            
            # Evaluate the expression using eval with restricted namespace
            allowed_names = {
                'sqrt': math.sqrt,
                'sin': lambda x: math.sin(math.radians(x)),
                'cos': lambda x: math.cos(math.radians(x)),
                'tan': lambda x: math.tan(math.radians(x)),
                'log': math.log10,
                'ln': math.log,
                'abs': abs,
                'pi': math.pi,
                'e': math.e,
            }
            
            result = eval(safe_expression, {"__builtins__": {}}, allowed_names)
            self._record_operation(expression, result)
            return result
        except ZeroDivisionError:
            raise ValueError("Cannot divide by zero!")
        except ValueError as e:
            raise ValueError(f"Invalid expression: {str(e)}")
        except Exception as e:
            raise ValueError(f"Error evaluating expression: {str(e)}")
    
    def _record_operation(self, operation: str, result: Union[int, float]) -> None:
        """Record operation in history."""
        self.history.append((operation, result))
    
    def get_history(self) -> List[Tuple[str, Union[int, float]]]:
        """Get calculation history."""
        return self.history.copy()
    
    def print_history(self) -> None:
        """Print calculation history in a formatted way."""
        if not self.history:
            print("No calculations performed yet.")
            return
        
        print("\n" + "="*50)
        print("CALCULATION HISTORY")
        print("="*50)
        for i, (operation, result) in enumerate(self.history, 1):
            print(f"{i}. {operation} = {result}")
        print("="*50 + "\n")
    
    def clear_history(self) -> None:
        """Clear calculation history."""
        self.history.clear()


def interactive_calculator():
    """Run the calculator in interactive mode."""
    calc = Calculator()
    
    print("\n" + "="*50)
    print("PYTHON CALCULATOR")
    print("="*50)
    print("\nBasic Operations: +, -, *, /, %, **")
    print("Scientific: sqrt, sin, cos, tan, log, ln, abs, factorial")
    print("Type 'history' to see calculation history")
    print("Type 'clear' to clear history")
    print("Type 'help' for detailed instructions")
    print("Type 'exit' to quit")
    print("="*50 + "\n")
    
    while True:
        try:
            user_input = input("Enter expression: ").strip()
            
            if user_input.lower() == 'exit':
                print("Thank you for using the calculator!")
                break
            elif user_input.lower() == 'history':
                calc.print_history()
            elif user_input.lower() == 'clear':
                calc.clear_history()
                print("History cleared.\n")
            elif user_input.lower() == 'help':
                print_help()
            elif user_input:
                result = calc.evaluate_expression(user_input)
                print(f"Result: {result}\n")
        
        except ValueError as e:
            print(f"Error: {e}\n")
        except Exception as e:
            print(f"Unexpected error: {e}\n")


def print_help():
    """Print help information."""
    help_text = """
    ╔════════════════════════════════════════════════════════╗
    ║                  CALCULATOR HELP                       ║
    ╚════════════════════════════════════════════════════════╝
    
    BASIC OPERATIONS:
    • Addition:        5 + 3
    • Subtraction:     10 - 4
    • Multiplication:  6 * 7
    • Division:        20 / 4
    • Modulus:         15 % 4
    • Power:           2 ** 8 or 2^8
    
    SCIENTIFIC FUNCTIONS:
    • Square Root:     sqrt(16)
    • Sine:            sin(90)        [angle in degrees]
    • Cosine:          cos(0)         [angle in degrees]
    • Tangent:         tan(45)        [angle in degrees]
    • Log (base 10):   log(100)
    • Natural Log:     ln(2.718)
    • Absolute Value:  abs(-5)
    • Factorial:       factorial(5) or 5!
    
    CONSTANTS:
    • Pi:              pi
    • Euler's Number:  e
    
    EXAMPLES:
    • (5 + 3) * 2
    • sqrt(144) + sin(90)
    • log(1000) / ln(e)
    • (2 ** 3) + (4 * 5)
    
    COMMANDS:
    • history  : View calculation history
    • clear    : Clear history
    • help     : Show this help message
    • exit     : Exit the calculator
    
    ╚════════════════════════════════════════════════════════╝
    """
    print(help_text)


if __name__ == "__main__":
    interactive_calculator()# Calculator
Scientific Calculator created by me with the help of GitHub copilot.
