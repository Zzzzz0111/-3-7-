class Train:
    def __init__(self, train_id, current_position, speed, status):
        self.train_id = train_id
        self.current_position = current_position
        self.speed = speed
        self.status = status

class TrainManager:
    def __init__(self):
        self.trains = []
        self.speed_threshold = 100  

    def add_train(self, train):
        self.trains.append(train)

    def get_train_status(self, train_id):
        for train in self.trains:
            if train.train_id == train_id:
                return train.status, train.current_position, train.speed
        return None, None, None

    def generate_report(self):
        report = {
            "trains": [{"train_id": train.train_id, "speed": train.speed, "status": train.status} for train in self.trains],
            "faulty_trains": [{"train_id": train.train_id, "status": train.status} for train in self.trains if train.status == "故障"],
            "average_speed": sum(train.speed for train in self.trains) / len(self.trains) if self.trains else 0
        }
        return report

    def check_speed_limit(self):
        for train in self.trains:
            if train.speed > self.speed_threshold:
                print(f"警报！列车 {train.train_id} 超速，当前速度 {train.speed} km/h，超过阈值 {self.speed_threshold} km/h！")

if __name__ == "__main__":
    manager = TrainManager()

    manager.add_train(Train("G101", "位置A", 80, "正常"))
    manager.add_train(Train("G102", "位置B", 120, "故障"))  
    manager.add_train(Train("G103", "位置C", 75, "已发车"))
    manager.add_train(Train("G104", "位置D", 90, "正常"))

    # 查询运行状态
    train_id = "G102"
    status, position, speed = manager.get_train_status(train_id)
    print(f"列车 {train_id} 的状态是：{status}，当前位置：{position}，速度：{speed} km/h")

    # 生成运行状态报告
    report = manager.generate_report()
    print("运行状态报告：")
    print("各个列车的速度和状态：", report["trains"])
    print("故障列车列表：", report["faulty_trains"])
    print("平均运行速度：", report["average_speed"], "km/h")

    # 检查超速列车并生成警报
    manager.check_speed_limit()
