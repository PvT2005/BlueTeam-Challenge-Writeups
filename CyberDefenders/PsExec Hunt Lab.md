# PsExec Hunt Lab — CyberDefenders Writeup

---

## Question 1. To effectively trace the attacker's activities within our network, can you identify the IP address of the machine from which the attacker initially gained access?

Go **Statistics → Conversations** and checked the IPv4 tab. The address `10.0.0.130` stands out — it's sending a large amount of traffic to both `10.0.0.131` and `10.0.0.133`, so that's the attacker's IP.

<img width="1582" height="612" alt="Image" src="https://github.com/user-attachments/assets/ef936bf9-e593-47f1-a1d9-55fcdfb76166" />

---

## Question 2. To fully understand the extent of the breach, can you determine the machine's hostname to which the attacker first pivoted?

Packet 132 contains an NTLMSSP_AUTH authentication from the attacker's IP `10.0.0.130`. Inside that packet I can see the target hostname — **Sales-PC**.

<img width="1797" height="697" alt="Image" src="https://github.com/user-attachments/assets/aa22e244-f64d-4df7-bc79-f5d7d97c0737" />

---

## Question 3. Knowing the username of the account the attacker used for authentication will give us insights into the extent of the breach. What is the username utilized by the attacker for authentication?

In the same packet 132, the username used for authentication is `ssales`.

<img width="1806" height="702" alt="Image" src="https://github.com/user-attachments/assets/c1985cf0-5510-45b0-972d-109250459e42" />

---

## Question 4. After figuring out how the attacker moved within our network, we need to know what they did on the target machine. What's the name of the service executable the attacker set up on the target?

The source machine then connects to the `ADMIN$` share successfully. This confirms that the `ssales` account has Administrator privileges on `.133`. In packets 144–145, the attacker pushes the service file `PSEXESVC.exe` (part of Microsoft's Sysinternals PsExec toolkit) to the `ADMIN$` directory on the target, and packet 146 kicks off the file transfer.

<img width="1806" height="702" alt="Image" src="https://github.com/user-attachments/assets/d30bf237-640f-4bf1-9017-1e86a672fa18" />

---

## Question 5. We need to know how the attacker installed the service on the compromised machine to understand the attacker's lateral movement tactics. This can help identify other affected systems. Which network share was used by PsExec to install the service on the target machine?

`ADMIN$` was used by PsExec to install the service on the target machine.

---

## Question 6. We must identify the network share used to communicate between the two machines. Which network share did PsExec use for communication?

`10.0.0.130` connects to the hidden share `IPC$` on the target. This is typically used to set up Named Pipes for RPC tasks, so `IPC$` is the share used for communication.

---

## Question 7. Now that we have a clearer picture of the attacker's activities on the compromised machine, it's important to identify any further lateral movement. What is the hostname of the second machine the attacker targeted to pivot within our network?

Same approach as Question 2 — I checked the NTLMSSP_AUTH packets and found the second target hostname is **Marketing-PC**.

<img width="1877" height="795" alt="Image" src="https://github.com/user-attachments/assets/366ff8b6-8161-41fa-8648-fcd1921ba6c6" />
