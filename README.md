# just_test

runner.py

from multiprocessing import Process, current_process, Pool

from worker import Worker
import time
import uuid

queue = []


def remove_item(result):
    print('remove', result)
    queue.remove(result)
    print('queue len after', len(queue))


if __name__ == '__main__':
    print('start main process')

    pool = Pool(processes=5)
    while True:
        if len(queue) >= 5:
            time.sleep(0.5)
            continue
        # 급등주 가져오기
        gen_uuid = uuid.uuid4()
        print('queue len before', len(queue))
        queue.append(gen_uuid)
        worker = Worker(gen_uuid)
        pool.apply_async(func=worker.run, callback=remove_item)


######################################
worker.py

import time


class Worker:
    def __init__(self, item_id, std_price):
        self.item_id = item_id
        self.std_price = std_price

    def run(self):
        # 1초마다
        print('check as', self.item_id)
        time.sleep(3)
        print('check success')
        print(self.item_id)
        return self.item_id
