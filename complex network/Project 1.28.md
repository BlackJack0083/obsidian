**Random Graphs with Clustering**

Abstract:
>The document presents a solution to the problem of modeling a network with clustering or transitivity, where two neighbors of a network node are also neighbors of one another, forming a triangle of connections in the network. This problem has proved difficult to model mathematically. The authors propose a generalized model of the standard 'configuration model' of network theory to create an ensemble model for which it is possible to calculate average properties exactly. The model that is proposed generalizes random graphs to incorporate clustering in a simple, sensible fashion, and derives exact formulas for many properties of the resulting networks. The authors show that this is possible, despite the fact that the triangles of clustered networks violate the condition that random graphs are 'locally treelike'. The paper includes mathematical equations and analyses that help calculate properties such as clustering coefficient, giant component size, and size of small components in the network.

**以前提出的网络聚类模型存在的主要问题是什么？**
以前提出的大多数网络聚类模型难以在数学上进行建模，无法以精确的方式计算网络的平均性质。现有的一些模型可能在某些特定情况下能够近似地表示网络的聚类，但不适用于大多数实际网络。此外，基于指数随机图的更一般的模型在大多数情况下难以解决，并且在其参数空间的某些部分存在问题。这些模型的性质计算通常只能依靠数值方法，缺乏精确的解析解。

**论文中的构造方式**
论文中提出的模型是通过广义化的“配置模型”来创造的。与传统的配置模型不同，这个广义模型是一个具有**任意度分布**的图模型，在每个顶点处不仅**指定了边的数量，还指定了三角形的数量**。
单独对待三角形内的边和单独存在的边。可以将单独的边看作*连接两个顶点的网络元素*，将三角形看作*连接三个顶点的不同类型的元素*。该模型可以进一步推广以包括四个或更多顶点的高阶元素。通过这个广义模型，可以准确计算出网络的许多属性
在本文提出的模型中，我们分别指定了每个顶点上的**单边和完整三角形**（阴影部分）的数量。

$$t_i: 顶点 \ i\ 参与的三角形数$$
$$s_i:顶点\ i \ 参与的除三角形以外的单边数量$$
**具体方法：**
通过随机选择 `stub`或者 `corners`来形成边或者三角形。具体来说，我们以均匀随机的方式选择一对 `stub` 来形成完全边，或者以随机方式选择一组三个 `corners` 来形成完整的三角形。最终的网络是从所有可能的 `stub` 和 `corner` 的匹配中均匀随机抽取的结果。**唯一的限制**是，为了确保在过程结束时没有剩余的 stub 或 corner，总 stub 的数量必须是2的倍数，总 corner 的数量必须是3的倍数。关于随机选择三角形角是指在构建网络时，我们以随机方式选择三个角（即 corners）并将它们相连形成一个完整的三角形。

>**度分布类别：**
>在网络科学中，度分布是描述网络中节点度数（连接数）分布的一种方式。度分布反映了网络中节点的度数是如何分布的，对于理解网络的结构和性质非常重要。以下是一些常见的度分布方式：
>1. **泊松分布（Poisson Distribution）：** 泊松分布是一种描述稀疏随机事件发生的概率分布。在网络科学中，如果网络的度分布服从泊松分布，那么大多数节点具有相似的度数，网络中存在的节点度数差异较小。
>2. **幂律分布（Power Law Distribution）：** 幂律分布是一种长尾分布，表示在网络中存在少量的节点拥有非常高的度数，而大多数节点的度数相对较小。幂律分布常用来描述复杂网络中的度分布，这种分布模式在许多实际网络中都很常见，如社交网络和互联网拓扑结构。
>3. **指数分布（Exponential Distribution）：** 指数分布是一种描述事件发生间隔的概率分布。在网络科学中，指数分布的度分布表明网络中大多数节点的度数相对较小，而高度连接的节点相对较少。
>4. **对数正态分布（Log-Normal Distribution）：** 对数正态分布是一种在对数尺度上呈正态分布的分布形式。在网络科学中，如果节点的度数在对数尺度上呈正态分布，那么大多数节点具有中等大小的度数，而极端度数的节点相对较少。
>5. **零膨胀分布（Zero-Inflated Distribution）：** 零膨胀分布是一种混合分布，用于描述网络中存在节点度数为零的情况。这在一些网络中是很常见的，例如社交网络中存在孤立的个体。
>
>这些分布方式提供了对不同类型网络中节点度数分布的建模方式，有助于深入理解网络的特性和行为。在实际应用中，网络科学研究者会根据具体网络的特点选择适当的度分布方式进行建模。

