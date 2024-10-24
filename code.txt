def divide(dividend, divisor):
    n = len(divisor)
    m = len(dividend)
    temp = dividend.copy()
    for i in range(m - n + 1):
        if temp[i] == 1:
            for j in range(n):
                temp[i + j] ^= divisor[j]
    return temp[-(n - 1):]

def check_error(received, divisor):
    remainder = divide(received, divisor)
    return any(remainder)

def sender():
    data = [1, 0, 0, 0, 1, 0, 0]
    divisor = [1, 1, 0, 1, 0]
    print("Original Data:", ''.join(map(str, data)))
    print("Divisor:", ''.join(map(str, divisor)))
    
    data += [0] * (len(divisor) - 1)  # Append zeros for CRC
    
    remainder = divide(data, divisor)
    code = data[:-(len(divisor) - 1)] + remainder
    
    print("Data sent with CRC appended:", ''.join(map(str, code)))
    print("CRC remainder:", ''.join(map(str, remainder)))
    
    return code, divisor

def receiver(sent_data):
    received_input = input("Enter the received data with CRC appended: ")
    
    if (received_input==sent_data):
        print("No Errors found in the received data.")
    else:
        print(" errors found in the received data.")

# Sender side: generates and prints the sent data with CRC
print("\nSender Side")
sent_data, divisor = sender()

# Manually input the sent data at the receiver side
print("\nReceiver Side")
receiver(sent_data, divisor)
