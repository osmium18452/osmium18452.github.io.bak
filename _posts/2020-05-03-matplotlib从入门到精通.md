---
layout:     post
title:      "matplotlib从入门到精通"
subtitle:   ""
date:       2020-05-03
author:     "lwb"
header-img: "img/post-bg-desk.jpg"
tags:
    - matplotlib
    - 画图
---

# 画图基本法

## 经典折线图

```
plt.figure()
x=range(len(a))
# plt.axis('scaled')
plt.xticks(x,lr)
plt.xlabel("Learning Rate")
plt.ylabel("Final Accuracy (%)")
plt.title("Final Accuracy")
kwargs = {
			"marker": "o",
			"lw":2
		}
plt.plot(x, a, label="SGD train accuracy", **kwargs)
# plt.legend()
sv = plt.gcf()
sv.savefig("./save/accuracy.png", format="png", dpi=100)
plt.show()
```

## 两个y轴的折线图

```
plt.figure()
ax1 = plt.subplot()
plt.title("loss and accuracy of training and validating")
ax1.set_xlabel("epochs")
x = range(len(train_ac))
ax1.set_ylabel("loss")
ax2 = ax1.twinx()
ax2.set_ylabel("accuracy")

kwargs = {
	"marker": None,
	"lw": 2,
}
l1, = ax1.plot(x, train_ls, color="tab:blue", label="train loss", **kwargs)
l3, = ax1.plot(x, valid_ls, color="tab:green", label="test loss", **kwargs)
l4, = ax2.plot(x, train_ac, color="tab:red", label="test accuracy", **kwargs)

plt.legend(handles=[l1, l3, l4], loc="center right")
sv = plt.gcf()
sv.savefig(os.path.join(args.save_dir, "lossAndAcc" + str(args.lr) +".png"), format="png", dpi=300)
```