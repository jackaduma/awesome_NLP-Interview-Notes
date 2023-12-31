#面试 #Attention 

- [[#一、seq2seq 篇|一、seq2seq 篇]]
		- [[#1.1 seq2seq （Encoder-Decoder）是什么？|1.1 seq2seq （Encoder-Decoder）是什么？]]
		- [[#1.2 seq2seq 中 的 Encoder 怎么样？|1.2 seq2seq 中 的 Encoder 怎么样？]]
		- [[#1.3 seq2seq 中 的 Decoder 怎么样？|1.3 seq2seq 中 的 Decoder 怎么样？]]
		- [[#1.4 在 数学角度上 的 seq2seq ，你知道么？|1.4 在 数学角度上 的 seq2seq ，你知道么？]]
		- [[#1.5 seq2seq 存在 什么 问题？|1.5 seq2seq 存在 什么 问题？]]
- [[#二、Attention 篇|二、Attention 篇]]
		- [[#2.1 什么是 Attention?|2.1 什么是 Attention?]]
		- [[#2.2 为什么引入 Attention机制？|2.2 为什么引入 Attention机制？]]
		- [[#2.3 Attention 有什么作用？|2.3 Attention 有什么作用？]]
		- [[#2.4 Attention 流程是怎么样？|2.4 Attention 流程是怎么样？]]
		- [[#2.5 Attention 的应用领域有哪些？|2.5 Attention 的应用领域有哪些？]]
- [[#三、Attention 变体篇|三、Attention 变体篇]]
		- [[#3.1 Soft Attention 是什么？|3.1 Soft Attention 是什么？]]
		- [[#3.2 Hard Attention 是什么？|3.2 Hard Attention 是什么？]]
		- [[#3.3 Global Attention 是什么？|3.3 Global Attention 是什么？]]
		- [[#3.4 Local Attention 是什么？|3.4 Local Attention 是什么？]]
		- [[#3.5 self-attention 是什么？|3.5 self-attention 是什么？]]

### 一、seq2seq 篇
##### 1.1 seq2seq （Encoder-Decoder）是什么？
Seq2seq（Sequence to Sequence），也被称为Encoder-Decoder模型，是一种用于序列到序列（sequence-to-sequence）任务的神经网络模型。

Seq2seq模型由两个主要的组成部分组成：编码器（Encoder）和解码器（Decoder）。

编码器负责将输入序列转换为固定长度的向量表示，通常是一个上下文向量（context vector）。它可以是一个RNN（如LSTM或GRU）或者是一系列卷积层的组合。编码器通过对输入序列逐步进行处理，将序列中的信息进行编码并捕捉上下文信息。

解码器负责根据编码器生成的上下文向量，逐步生成目标序列。解码器也可以是一个RNN，它通过上一个时间步的隐藏状态和当前的输入来预测下一个时间步的输出。解码器在每个时间步都会生成一个单词或标记，直到生成完整的目标序列。

Seq2seq模型在机器翻译、文本摘要、对话生成等任务中得到广泛应用。它能够处理变长的输入和输出序列，并且能够捕捉到序列之间的关联信息。通过训练，模型可以学习到输入序列和输出序列之间的映射关系，从而实现序列到序列的转换。
##### 1.2 seq2seq 中 的 Encoder 怎么样？
在序列到序列（seq2seq）模型中，Encoder是负责将输入序列编码为一个固定长度的向量表示的部分。Encoder通常使用循环神经网络（RNN）或者其变种（如长短时记忆网络，LSTM）来处理输入序列。

Encoder的工作流程如下：

1. 输入序列中的每个单词或字符经过嵌入层转换为稠密向量表示。

2. 这些向量作为输入被逐个输入到RNN中。RNN的隐藏状态在每个时间步被更新，以便捕捉序列中的上下文信息。

3. 最后一个时间步的隐藏状态被视为整个输入序列的编码表示。这个向量包含了输入序列的信息，并且被用作Decoder的初始隐藏状态。

Encoder的目标是将输入序列的信息编码为一个固定长度的向量，这个向量应该包含足够的信息以便Decoder能够生成正确的输出序列。通过训练，Encoder学习到了如何将输入序列的信息转化为有用的表示。

需要注意的是，Encoder的设计可以有多种变体。例如，可以使用双向RNN来处理输入序列，以便同时考虑前向和后向的上下文信息。此外，可以使用注意力机制来加强Encoder的表示能力，使其能够更好地捕捉输入序列中的重要信息。

总之，Encoder在seq2seq模型中起到了将输入序列编码为固定长度向量表示的关键作用，为Decoder提供了有用的上下文信息，从而实现了序列到序列的翻译、摘要等任务。
##### 1.3 seq2seq 中 的 Decoder 怎么样？
在序列到序列（seq2seq）模型中，Decoder负责将编码后的输入序列解码为输出序列。Decoder通常也使用循环神经网络（RNN）或其变种（如长短时记忆网络，LSTM）来处理输出序列。

Decoder的工作流程如下：

1. 初始隐藏状态：Decoder的初始隐藏状态通常使用Encoder的最后一个时间步的隐藏状态来初始化。这个隐藏状态包含了输入序列的信息，并且作为Decoder的起点。

2. 输入处理：在每个时间步，Decoder接收到上一个时间步的输出以及上一个时间步的隐藏状态作为输入。这些输入经过嵌入层转换为稠密向量表示。

3. 隐藏状态更新：输入向量和上一个时间步的隐藏状态被输入到RNN中，RNN的隐藏状态在每个时间步被更新，以便捕捉输出序列的上下文信息。

4. 输出预测：RNN的隐藏状态被用于预测当前时间步的输出。输出通常通过一个全连接层映射到词汇表的维度，并经过softmax函数得到概率分布。

5. 生成下一个时间步的输入：预测的输出通常被用作下一个时间步的输入，这个过程会不断重复，直到生成完整的输出序列或达到最大长度。

Decoder的目标是根据输入序列的编码表示和上一个时间步的输出来生成下一个时间步的输出，从而逐步生成完整的输出序列。通过训练，Decoder学习到了如何根据上下文信息生成合适的输出。

与Encoder类似，Decoder的设计也可以有多种变体。例如，可以使用注意力机制来加强Decoder的表示能力，使其能够更好地对输入序列的不同部分进行关注。此外，可以使用束搜索（beam search）等技术来生成更准确的输出序列。

总之，Decoder在seq2seq模型中起到了将编码后的输入序列解码为输出序列的关键作用，通过逐步生成输出，实现了序列到序列的翻译、摘要等任务。
##### 1.4 在 数学角度上 的 seq2seq ，你知道么？
在数学角度上，seq2seq模型可以被视为一个**条件概率分布建模**的问题。它可以被表示为一个条件语言模型，其中给定输入序列的条件下，模型需要学习输出序列的条件概率分布。

具体地，seq2seq模型可以看作是一个由两个RNN组成的条件概率模型，其中一个RNN作为Encoder，负责将输入序列编码为一个固定长度的向量表示，另一个RNN作为Decoder，负责根据Encoder的输出和上一个时间步的输出来生成下一个时间步的输出。

数学上，可以将Encoder表示为函数f，将Decoder表示为函数g。给定输入序列x和输出序列y，seq2seq模型的目标是最大化条件概率$P(y|x)$，即：

$P(y|x) = ∏ P(y_t|y_1,...,y_{t-1},x)$

其中，$y_t$表示输出序列的第t个元素。通过训练，模型需要学习到Encoder和Decoder的参数，使得在给定输入序列x的条件下，生成的输出序列y的概率最大。

在数学角度上，seq2seq模型的训练可以通过最大似然估计来实现，即最大化训练数据中观测到的输出序列的条件概率。常用的优化算法如随机梯度下降（SGD）和其变种（如Adam）可以用于更新模型的参数。

总之，从数学角度上看，seq2seq模型可以被视为一个条件概率模型，通过最大化条件概率来学习如何将输入序列映射到输出序列。
##### 1.5 seq2seq 存在 什么 问题？
Seq2seq模型存在一些问题，其中一些主要问题如下：

1. 缺乏对长距离依赖的建模能力：传统的RNN在处理长序列时容易出现梯度消失或梯度爆炸的问题，导致难以捕捉长距离依赖关系。这使得seq2seq模型在处理长句子或长文本时表现不佳。

2. 信息压缩：Encoder将整个输入序列编码为一个固定长度的向量表示，这可能会导致信息的丢失或压缩。当输入序列较长或含有大量细节时，Encoder可能无法有效地将所有信息编码到向量中。

3. 缺乏全局上下文：Decoder在生成输出序列时，只能依赖于Encoder的向量表示和上一个时间步的输出。这可能导致Decoder无法充分利用输入序列的全局上下文信息。

4. 生成偏置：在生成输出序列时，Decoder往往倾向于生成常见的词汇或短语，而忽略一些罕见或复杂的词汇。这可能导致生成的序列过于保守或缺乏多样性。

5. 训练困难：由于输出序列的长度通常与输入序列不同，因此在训练时，需要使用特殊的技术（如Teacher Forcing）来指导Decoder生成正确的输出。这可能导致训练过程不稳定或困难。

为了解决这些问题，一些改进的模型和技术被提出，如使用注意力机制来增强Decoder的表示能力和对输入序列的关注，使用更复杂的RNN结构（如LSTM、GRU）来处理长序列，以及使用更先进的优化算法和训练技巧来提高训练效果。
### 二、Attention 篇
##### 2.1 什么是 Attention?
注意力机制（Attention）是一种用于增强模型对输入序列中不同位置的关注程度的技术。在序列到序列（seq2seq）模型中，注意力机制被广泛应用于提高模型的性能。

在传统的seq2seq模型中，Encoder将整个输入序列编码为一个固定长度的向量表示，然后Decoder使用该向量来生成输出序列。然而，这种方法存在**信息压缩**和**缺乏全局上下文**的问题。

注意力机制通过引入额外的注意力权重，使得Decoder在生成每个输出元素时，可以在输入序列的不同位置分配不同的注意力权重。这样，Decoder可以根据输入序列的不同部分调整其关注程度，更好地利用输入序列的全局上下文信息。

具体来说，注意力机制通过以下步骤实现：

1. Encoder将输入序列编码为一系列隐藏状态（或向量表示）。
2. Decoder在生成每个输出元素时，通过计算与输入序列中每个隐藏状态之间的相似度（通常使用点积或其他函数），得到一个注意力权重向量。
3. 注意力权重向量经过归一化处理，得到每个输入位置的注意力权重。
4. 通过对每个输入位置的隐藏状态和对应的注意力权重进行加权求和，得到一个加权向量表示，作为Decoder的上下文向量。
5. Decoder使用上下文向量作为额外输入，来生成下一个输出元素。

通过引入注意力机制，模型可以根据输入序列的不同部分**动态调整关注程度**，从而更好地捕捉输入序列的重要信息。这有助于提高模型的性能，特别是在处理长序列或具有复杂关系的序列时。
##### 2.2 为什么引入 Attention机制？
引入注意力机制的目的是解决传统的seq2seq模型中存在的一些问题，包括信息压缩、缺乏全局上下文和处理长序列的能力不足等。

1. 信息压缩：传统的seq2seq模型将整个输入序列编码为一个固定长度的向量表示，这可能导致信息的丢失或压缩。引入注意力机制后，Decoder可以根据输入序列的不同部分调整关注程度，从而更好地利用输入序列的全局信息，避免了信息的压缩和丢失。

2. 缺乏全局上下文：传统的seq2seq模型在生成输出序列时，只能依赖于Encoder的向量表示和上一个时间步的输出。这可能导致Decoder无法充分利用输入序列的全局上下文信息。通过引入注意力机制，Decoder可以根据输入序列的不同部分动态调整关注程度，从而更好地捕捉输入序列的重要信息，提供更丰富的全局上下文。

3. 处理长序列的能力不足：传统的RNN在处理长序列时容易出现梯度消失或梯度爆炸的问题，导致难以捕捉长距离依赖关系。注意力机制通过允许Decoder在生成每个输出元素时，根据输入序列的不同部分调整关注程度，从而可以更好地处理长序列，捕捉长距离依赖关系。

总的来说，引入注意力机制可以提供更好的建模能力和全局上下文，允许模型根据输入序列的重要性调整关注程度，从而提高模型的性能，特别是在处理长序列或具有复杂关系的序列时。
##### 2.3 Attention 有什么作用？
注意力机制（Attention）在机器学习和自然语言处理等领域中具有重要的作用，主要体现在以下几个方面：

1. 提高模型性能：引入注意力机制可以提高模型的性能，特别是在处理长序列或具有复杂关系的序列时。通过动态调整关注程度，模型可以更好地利用输入序列的全局上下文信息，捕捉重要的输入部分，从而改善模型的预测能力。

2. 解决信息压缩问题：传统的seq2seq模型将整个输入序列编码为一个固定长度的向量表示，可能导致信息的丢失或压缩。引入注意力机制后，模型可以根据输入序列的不同部分调整关注程度，避免了信息的压缩和丢失，提高了模型的表达能力。

3. 捕捉全局上下文：传统的seq2seq模型在生成输出序列时，只能依赖于固定长度的向量表示和上一个时间步的输出。引入注意力机制后，模型可以根据输入序列的不同部分动态调整关注程度，捕捉输入序列的全局上下文信息，提供更丰富的上下文，改善模型的生成能力。

4. 处理长序列：传统的RNN在处理长序列时容易出现梯度消失或梯度爆炸的问题，导致难以捕捉长距离依赖关系。注意力机制通过允许模型根据输入序列的不同部分调整关注程度，可以更好地处理长序列，捕捉长距离依赖关系，提高模型的序列建模能力。

总的来说，注意力机制可以提高模型的性能，改善模型的表达能力和生成能力，同时解决信息压缩问题和处理长序列的困难，从而在各种任务中发挥重要作用。
##### 2.4 Attention 流程是怎么样？
Attention机制的流程通常包括以下几个步骤：

1. 编码器（Encoder）：首先，输入序列经过编码器进行编码，得到一系列的隐藏状态或向量表示。编码器可以是任何序列模型，如RNN、Transformer等。

2. 注意力权重计算：然后，根据编码器的输出和解码器（Decoder）的当前状态，计算注意力权重。注意力权重表示了解码器对编码器输出的关注程度，决定了编码器输出在解码器中的重要性。

3. 上下文向量计算：根据注意力权重和编码器的输出，计算上下文向量。上下文向量是编码器输出在注意力机制下的加权平均或加权求和，用于传递编码器的重要信息给解码器。

4. 解码器输出计算：将上下文向量和解码器的当前状态作为输入，计算解码器的输出。解码器可以是任何序列模型，如RNN、Transformer等。

5. 重复步骤2-4：重复进行步骤2-4，直到生成完整的输出序列或达到预定的停止条件。

在计算注意力权重时，常用的方法有多种，如点积注意力、加性注意力、缩放点积注意力等。具体的计算方式取决于注意力机制的设计和使用的模型架构。

需要注意的是，Attention机制的流程可以根据具体的应用和模型进行调整和扩展，上述流程只是一个基本的框架。不同的模型和任务可能会有一些差异和变化。
- 步骤一  执行encoder (与 seq2seq 一致)
- 步骤二  计算对齐系数 a
- 步骤三  计算上下文语义向量 C
- 步骤四  更新decoder状态
- 步骤五  计算输出预测词
##### 2.5 Attention 的应用领域有哪些？
Attention机制在许多领域中都有广泛的应用，包括但不限于以下几个方面：

1. 机器翻译：Attention机制在机器翻译任务中被广泛应用。通过引入注意力机制，模型可以根据源语言句子的不同部分动态调整关注程度，从而更好地捕捉源语言和目标语言之间的对应关系，提高翻译质量。

2. 文本摘要：Attention机制在文本摘要任务中也有重要应用。通过引入注意力机制，模型可以根据输入文本的不同部分动态调整关注程度，从而更好地捕捉文本中的关键信息，生成准确、有信息量的摘要。

3. 语音识别：Attention机制在语音识别任务中扮演着重要角色。通过引入注意力机制，模型可以根据输入语音的不同部分动态调整关注程度，从而更好地捕捉语音信号中的关键信息，提高识别准确率。

4. 图像描述生成：Attention机制在图像描述生成任务中也得到了广泛应用。通过引入注意力机制，模型可以根据图像的不同区域动态调整关注程度，从而更好地捕捉图像中的重要内容，生成准确、具有描述性的文本。

5. 问答系统：Attention机制在问答系统中也有应用。通过引入注意力机制，模型可以根据问题和文档的不同部分动态调整关注程度，从而更好地定位和关联问题与答案之间的相关信息，提高问答准确率。

除了上述应用领域，Attention机制还可以用于情感分析、命名实体识别、语义角色标注等自然语言处理任务，以及图像分类、目标检测、图像生成等计算机视觉任务中。总的来说，Attention机制在各种任务中都能够提供更好的建模能力和性能。
### 三、Attention 变体篇
##### 3.1 Soft Attention 是什么？
Soft Attention是一种**基于概率分布**的注意力机制，用于在序列模型中动态地计算输入序列中不同位置的权重。Soft Attention通过计算每个输入位置的注意力权重，将输入序列的不同部分对模型输出的贡献进行建模。

Soft Attention的计算过程通常包括以下几个步骤：

1. 计算注意力权重：首先，根据模型的当前状态和输入序列中的每个位置，计算该位置对模型输出的注意力权重。注意力权重通常通过计算输入位置与模型状态之间的相似度来得到。

2. 归一化注意力权重：将注意力权重进行归一化，使其总和为1，以确保注意力权重是一个有效的概率分布。

3. 加权求和：根据归一化后的注意力权重，对输入序列的每个位置进行加权求和。这样可以根据注意力权重来动态地计算输入序列的加权平均或加权求和，得到一个上下文向量。

4. 上下文向量的应用：将上下文向量与模型的当前状态或其他信息进行结合，用于后续的模型计算或决策。

Soft Attention的优点是能够在不同的输入位置上分配不同的权重，从而更好地捕捉输入序列中的重要信息。Soft Attention可以应用于各种序列模型，如循环神经网络（RNN）、长短时记忆网络（LSTM）、门控循环单元（GRU）以及自注意力模型（如Transformer）。
##### 3.2 Hard Attention 是什么？
Hard Attention是一种**基于离散选择**的注意力机制，与Soft Attention相对。在Hard Attention中，模型不再对输入序列的每个位置计算一个连续的注意力权重，而是通过选择一个或多个位置来获得注意力。

Hard Attention的计算过程通常包括以下几个步骤：

1. 选择注意力位置：根据模型的当前状态和输入序列，**通过某种策略**选择一个或多个位置作为注意力位置。选择过程可以基于一些规则或者采用一些采样方法，例如使用随机采样或者基于概率分布的采样。

2. 获取注意力权重：根据选择的注意力位置，获取对应位置的注意力权重。这些权重可以是固定的，也可以是根据某种规则或概率分布计算得到的。

3. 加权求和：根据注意力权重对选择的位置进行加权求和，得到一个上下文向量。

4. 上下文向量的应用：将上下文向量与模型的当前状态或其他信息进行结合，用于后续的模型计算或决策。

与Soft Attention相比，Hard Attention的优点是能够通过离散选择来减少计算量，并且可以更加明确地指定模型关注的位置。然而，Hard Attention也存在一些缺点，例如难以训练和优化，以及无法直接对注意力权重进行梯度反向传播等。因此，Soft Attention在实践中更为常用。
##### 3.3 Global Attention 是什么？
Global Attention是一种注意力机制，用于在序列模型中**对整个输入序列的信息进行建模和捕捉**。与Soft Attention和Hard Attention不同，Global Attention不仅考虑输入序列中各个位置的注意力权重，还考虑了整个输入序列的重要性。

在Global Attention中，注意力权重的计算过程通常包括以下几个步骤：

1. 计算注意力分数：根据模型的当前状态和输入序列中的每个位置，计算该位置对模型输出的注意力分数。注意力分数通常通过计算输入位置与模型状态之间的相似度来得到。

2. 归一化注意力分数：将注意力分数进行归一化，使其总和为1，以确保注意力分数是一个有效的概率分布。

3. 加权求和：根据归一化后的注意力分数，对输入序列的每个位置进行加权求和。这样可以根据注意力分数来动态地计算输入序列的加权平均或加权求和，得到一个上下文向量。

4. 上下文向量的应用：将上下文向量与模型的当前状态或其他信息进行结合，用于后续的模型计算或决策。

Global Attention在计算注意力权重时，考虑了整个输入序列的信息，因此能够更全面地捕捉输入序列的重要性。它可以应用于各种序列模型，如循环神经网络（RNN）、长短时记忆网络（LSTM）、门控循环单元（GRU）以及自注意力模型（如Transformer）。
##### 3.4 Local Attention 是什么？
Local Attention是一种注意力机制，用于在序列模型中对输入序列中的局部信息进行建模和捕捉。与Global Attention不同，Local Attention仅关注输入序列中与当前模型状态相关的一部分位置，而忽略其他位置的信息。

在Local Attention中，注意力权重的计算过程通常包括以下几个步骤：

1. 计算注意力分数：根据模型的当前状态和输入序列中的每个位置，计算该位置对模型输出的注意力分数。与Global Attention不同的是，Local Attention只计算与当前模型状态相关的一部分位置的注意力分数。

2. 归一化注意力分数：将注意力分数进行归一化，使其总和为1，以确保注意力分数是一个有效的概率分布。

3. 加权求和：根据归一化后的注意力分数，对与当前模型状态相关的输入序列位置进行加权求和。这样可以根据注意力分数来动态地计算局部输入序列的加权平均或加权求和，得到一个上下文向量。

4. 上下文向量的应用：将上下文向量与模型的当前状态或其他信息进行结合，用于后续的模型计算或决策。

Local Attention在计算注意力权重时，只关注与当前模型状态相关的一部分位置，从而减少了计算和存储的复杂度。这种注意力机制在处理长序列时，能够更加高效地捕捉输入序列中的局部信息。它可以应用于各种序列模型，如循环神经网络（RNN）、长短时记忆网络（LSTM）、门控循环单元（
##### 3.5 self-attention 是什么？
Self-attention（自注意力）是一种注意力机制，用于在序列模型中**对序列中的不同位置之间**建立关联关系。它是Transformer模型中的关键组成部分。

在self-attention中，输入序列中的每个位置都被视为一个查询（query）、一个键（key）和一个值（value）。通过计算查询和键之间的相似度，可以得到每个位置对其他位置的注意力权重。然后，根据这些注意力权重对值进行加权求和，得到一个上下文向量。

具体而言，self-attention的计算过程如下：

1. 查询、键和值的计算：对于输入序列中的每个位置，通过线性变换将其映射为查询、键和值。这些映射可以使用不同的权重矩阵进行计算。

2. 相似度计算：计算查询和键之间的相似度，通常使用点积（dot product）或其他类似的方法。相似度越高，注意力权重越大。

3. 注意力权重的计算：将相似度进行缩放和归一化，得到注意力权重。这些权重表示了每个位置对其他位置的重要性。

4. 加权求和：根据注意力权重对值进行加权求和，得到一个上下文向量。这个上下文向量包含了序列中不同位置之间的关联信息。

Self-attention的优势在于它能够对序列中的不同位置之间建立动态的关联关系，而不受序列长度的限制。它可以捕捉到序列中的长距离依赖关系，从而提高模型的性能。在自然语言处理任务中，self-attention在机器翻译、文本生成等任务中取得了很好的效果。