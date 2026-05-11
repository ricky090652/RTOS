# RTOS Labs: uC/OS-II Scheduling & Synchronization

## LAB 1: Rate Monotonic (RM) Scheduling
*   **目的**：實作靜態優先級排程演算法。
*   **實作**：
    *   **TCB 擴充**：在 `OS_TCB` 結構中加入任務週期（Period）與執行時間（Computation Time）等欄位。
    *   **週期管理**：修改 `OSTimeTick()`，在每個 Tick會把所有TCB delay counter 減 1，當計數器變為0時，就把該任務加ready list，並且每個time tick把comptime減一。
    *   **任務實作 (test.c)**：任務進入 `while(1)` 迴圈，以忙碌等待（Busy-waiting）模擬運算。運算完成後計算距離下個週期的剩餘時間，並呼叫 `OSTimeDly()` 進入等待，直到被重新排程。


## LAB 2: Earliest Deadline First (EDF) Scheduling
*   **目的**：實作動態優先級排程演算法。
*   **實作**：
    *   在 `OS_Sched()` 與 `OSIntExit()` 把原本更新優先權的函式，改成自定義的 `OS_SchedEDF()` 函式。
    *   動態計算任務的 Deadline，並選擇最接近截止時間的任務執行。
    *   解決 Deadline 相同時的 Tie-breaking 問題（優先保留當前任務）。

## LAB 3: Ceiling Priority Protocol (CPP)
*   **目的**：透過mutex locks實作CPP Protocol(resource synchronization)。
*   **實作**：
    *   修改 `OSMutexPend()`：當任務取得 Mutex 時，立即將其優先級提升至該資源的 Ceiling Priority。
    *   修改 `OSMutexPost()`：釋放 Mutex 後，將任務恢復至原始優先級。
    *   確保在巢狀 Mutex（Nested Mutex）情境下優先級管理的正確性。
