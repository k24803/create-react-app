import React, { useState, useEffect } from 'react';

// Giả lập dữ liệu sản phẩm của MissCandle
// Đã cập nhật với thông tin từ tất cả các hình ảnh bạn cung cấp
const products = [
  {
    id: '1',
    name: 'Nến Thơm Bình Yên',
    image: 'https://placehold.co/400x300/F0F0F0/333333?text=Binh+Yen',
    description: 'Hương oải hương và gỗ đàn hương, mang lại cảm giác thư thái, tĩnh lặng cho không gian của bạn. Lý tưởng cho những buổi tối thiền định hoặc đọc sách.',
    story: 'Mùi hương này được lấy cảm hứng từ những khoảnh khắc bình yên nhất trong cuộc sống, khi bạn được là chính mình và tìm thấy sự an nhiên trong tâm hồn.',
    emotions: ['bình yên', 'thư giãn', 'tĩnh lặng', 'thiền định', 'ấm áp', 'ấm cúng'],
    style: ['tối giản', 'tự nhiên', 'ấm cúng'],
    price: '280.210 ₫',
    originalPrice: '549.000 ₫',
    weight: '340 gram',
    burnTime: 'N/A', // Not specified in images for this general product
    scentNotes: ['Oải hương', 'Gỗ đàn hương'],
    scentProfiles: ['floral', 'woody', 'calming'],
    musicSuggestions: ['Nhạc thiền', 'Acoustic nhẹ nhàng', 'Lo-fi chill']
  },
  {
    id: '2',
    name: 'Nến Thơm Năng Lượng',
    image: 'https://placehold.co/400x300/F0F0F0/333333?text=Nang+Luong',
    description: 'Kết hợp hương cam quýt tươi mát và bạc hà sảng khoái, giúp đánh thức giác quan và tăng cường sự tập trung. Hoàn hảo cho buổi sáng hoặc khi làm việc.',
    story: 'Lấy cảm hứng từ ánh nắng ban mai và sự khởi đầu tươi mới, Nến Thơm Năng Lượng là nguồn động lực để bạn bắt đầu một ngày đầy hứng khởi.',
    emotions: ['năng lượng', 'tập trung', 'sảng khoái', 'tươi mới', 'tỉnh táo'],
    style: ['hiện đại', 'năng động'],
    price: '280.210 ₫',
    originalPrice: '549.000 ₫',
    weight: '340 gram',
    burnTime: 'N/A',
    scentNotes: ['Cam quýt', 'Bạc hà'],
    scentProfiles: ['citrus', 'fresh', 'energizing'],
    musicSuggestions: ['Pop sôi động', 'Indie Pop', 'Nhạc điện tử vui tươi']
  },
  {
    id: '3',
    name: 'Nến Thơm Lãng Mạn',
    image: 'https://placehold.co/400x300/F0F0F0/333333?text=Lang+Man',
    description: 'Hương hoa hồng quyến rũ và vani ngọt ngào, tạo không gian lãng mạn và ấm áp. Tuyệt vời cho những buổi hẹn hò hoặc kỷ niệm.',
    story: 'Được tạo ra để tôn vinh tình yêu và những khoảnh khắc đáng nhớ, mùi hương này sẽ là chất xúc tác cho những cảm xúc thăng hoa và ngọt ngào.',
    emotions: ['lãng mạn', 'tình yêu', 'ngọt ngào', 'ấm áp', 'quyến rũ', 'hẹn hò'],
    style: ['sang trọng', 'lãng mạn', 'cổ điển'],
    price: '280.210 ₫',
    originalPrice: '549.000 ₫',
    weight: '340 gram',
    burnTime: 'N/A',
    scentNotes: ['Hoa hồng', 'Vani'],
    scentProfiles: ['floral', 'sweet', 'romantic'],
    musicSuggestions: ['Ballad tình yêu', 'Nhạc cổ điển lãng mạn', 'Soft R&B']
  },
  {
    id: '4',
    name: 'Nến Thơm Thanh Lọc',
    image: 'https://placehold.co/400x300/F0F0F0/333333?text=Thanh+Loc',
    description: 'Hương trà xanh và tre tươi mát, giúp thanh lọc không khí và mang lại cảm giác trong lành, tinh khiết. Thích hợp cho mọi không gian.',
    story: 'Mùi hương của sự tinh khiết và đổi mới, giúp bạn gột rửa những lo toan và đón nhận nguồn năng lượng tích cực, trong lành.',
    emotions: ['thanh lọc', 'trong lành', 'tinh khiết', 'sạch sẽ', 'tươi mát'],
    style: ['tối giản', 'hiện đại', 'tự nhiên'],
    price: '280.210 ₫',
    originalPrice: '549.000 ₫',
    weight: '340 gram',
    burnTime: 'N/A',
    scentNotes: ['Trà xanh', 'Tre'],
    scentProfiles: ['fresh', 'green', 'clean'],
    musicSuggestions: ['Nhạc Ambient', 'Nhạc không lời thư giãn', 'Chillwave']
  },
  {
    id: '5',
    name: 'Nến Thơm Mộc Mạc',
    image: 'https://placehold.co/400x300/F0F0F0/333333?text=Moc+Mac',
    description: 'Hương gỗ thông và hổ phách ấm áp, gợi nhớ về thiên nhiên hoang dã và sự mộc mạc, chân thật. Dành cho những ai yêu thích sự giản dị.',
    story: 'Lấy cảm hứng từ những khu rừng sâu thẳm và vẻ đẹp nguyên sơ của thiên nhiên, mùi hương này đưa bạn trở về với sự mộc mạc và chân thật nhất của bản thân.',
    emotions: ['mộc mạc', 'thiên nhiên', 'ấm áp', 'chân thật', 'giản dị', 'yên bình'],
    style: ['vintage', 'tự nhiên', 'ấm cúng'],
    price: '280.210 ₫',
    originalPrice: '549.000 ₫',
    weight: '340 gram',
    burnTime: 'N/A',
    scentNotes: ['Gỗ thông', 'Hổ phách'],
    scentProfiles: ['woody', 'warm', 'natural'],
    musicSuggestions: ['Nhạc đồng quê', 'Blues', 'Folk acoustic']
  },
  // Sản phẩm từ hình ảnh được cung cấp trước đó
  {
    id: '6',
    name: 'Vườn Xuân Ngọt Ngào (Warm Spring Sunshine)',
    image: 'uploaded:IMG_4885.PNG-bc92d81e-c17a-4c65-8223-8dec711d9136',
    description: 'Hương thơm ngọt ngào của đào trắng, táo và dưa lưới, kết hợp với hoa cúc và hổ phách, mang đến cảm giác tươi mới của mùa xuân.',
    story: 'Đắm mình trong không gian ngập tràn nắng ấm và hương hoa trái ngọt lành, như một buổi dạo chơi trong vườn xuân rực rỡ.',
    emotions: ['tươi mới', 'vui vẻ', 'ngọt ngào', 'sảng khoái'],
    style: ['tươi sáng', 'nữ tính', 'hiện đại'],
    price: '280.210 ₫',
    originalPrice: '549.000 ₫',
    weight: '340 gram',
    burnTime: '20-25h',
    scentNotes: ['Đào trắng', 'Táo', 'Dưa lưới', 'Hoa Cúc', 'Hổ Phách'],
    scentProfiles: ['fruity', 'floral', 'sweet', 'warm'],
    musicSuggestions: ['Pop mùa hè', 'Acoustic vui tươi', 'Indie Pop']
  },
  {
    id: '7',
    name: 'Kem Dừa Vani Béo (Salted Coconut & Mahogany)',
    image: 'uploaded:IMG_4886.PNG-609963f8-f05d-4f0f-be38-f0bcecc198d2',
    description: 'Sự kết hợp độc đáo giữa cam chanh, thảo mộc, gỗ mahogany, vani và dừa, tạo nên một mùi hương béo ngậy, ấm áp và quyến rũ.',
    story: 'Gợi nhớ về những kỳ nghỉ nhiệt đới, nơi bạn được thư giãn bên ly cocktail dừa và tận hưởng hương thơm ngọt ngào, ấm cúng.',
    emotions: ['ngọt ngào', 'ấm áp', 'quyến rũ', 'thư giãn'],
    style: ['sang trọng', 'ấm cúng', 'độc đáo'],
    price: '280.210 ₫',
    originalPrice: '549.000 ₫',
    weight: '340 gram',
    burnTime: '20-25h',
    scentNotes: ['Cam Chanh', 'Thảo mộc', 'Gỗ Mahogany', 'Vani', 'Dừa'],
    scentProfiles: ['gourmand', 'sweet', 'woody', 'tropical'],
    musicSuggestions: ['Smooth Jazz', 'Lounge music', 'R&B nhẹ nhàng']
  },
  {
    id: '8',
    name: 'Đà Lạt Tươi Mát (Turkish Cotton)',
    image: 'uploaded:IMG_4887.PNG-36132134-dd54-490e-b6fd-23a5dec2cc33',
    description: 'Hương hoa bông gòn, hổ phách, xạ hương, gỗ tuyết tùng và vani, mang đến cảm giác trong lành, tinh khiết như không khí Đà Lạt.',
    story: 'Mùi hương của sự tinh khôi và bình yên, như một buổi sáng sớm tại Đà Lạt với sương mù và không khí trong lành.',
    emotions: ['trong lành', 'tinh khiết', 'bình yên', 'thư thái'],
    style: ['tối giản', 'tự nhiên', 'thanh lịch'],
    price: '280.210 ₫',
    originalPrice: '549.000 ₫',
    weight: '340 gram',
    burnTime: '20-25h',
    scentNotes: ['Hoa Bông Gòn', 'Hổ Phách', 'Xạ Hương', 'Gỗ Tuyết Tùng', 'Vani'],
    scentProfiles: ['fresh', 'clean', 'woody', 'soft'],
    musicSuggestions: ['Nhạc không lời', 'Ambient', 'Nhạc thư giãn']
  },
  {
    id: '9',
    name: 'Khu Rừng Bí Ẩn (Warm Leather & Amber)',
    image: 'uploaded:IMG_4888.PNG-004ac901-1d1a-494c-b29d-88ef433c456a',
    description: 'Hương hổ phách, trầm hương, gỗ mahogany, vani khô và cacao, tạo nên một không gian ấm áp, bí ẩn và đầy lôi cuốn.',
    story: 'Đắm chìm vào không gian huyền bí của một khu rừng cổ thụ, nơi hương gỗ trầm ấm hòa quyện cùng sự ngọt ngào bí ẩn.',
    emotions: ['bí ẩn', 'ấm áp', 'sang trọng', 'lôi cuốn', 'thư thái'],
    style: ['cổ điển', 'sang trọng', 'mạnh mẽ'],
    price: '280.210 ₫',
    originalPrice: '549.000 ₫',
    weight: '340 gram',
    burnTime: '20-25h',
    scentNotes: ['Hổ Phách', 'Trầm Hương', 'Gỗ Mahogany', 'Vani khô', 'Cacao'],
    scentProfiles: ['woody', 'amber', 'warm', 'spicy', 'gourmand'],
    musicSuggestions: ['Jazz cổ điển', 'Blues', 'Nhạc phim mystery', 'Lo-fi beats']
  },
  {
    id: '10',
    name: 'Trái Cây Nhiệt Đới (Red Lava & Citrus)',
    image: 'uploaded:IMG_4889.PNG-840f1107-5e91-439b-8617-8c3a451fb23a',
    description: 'Sự bùng nổ của xoài, quýt, nho đỏ, cam đỏ và lá chanh, mang đến một mùi hương tươi mát, sảng khoái và đầy năng lượng của vùng nhiệt đới.',
    story: 'Hãy để mùi hương này đưa bạn đến một hòn đảo nhiệt đới đầy nắng, nơi những loại trái cây tươi ngon đang chờ đón.',
    emotions: ['tươi mát', 'sảng khoái', 'năng lượng', 'vui vẻ'],
    style: ['năng động', 'trẻ trung', 'tươi sáng'],
    price: '280.210 ₫',
    originalPrice: '549.000 ₫',
    weight: '340 gram',
    burnTime: '20-25h',
    scentNotes: ['Xoài', 'Quýt', 'Nho Đỏ', 'Cam Đỏ', 'Lá Chanh'],
    scentProfiles: ['fruity', 'citrus', 'tropical', 'energizing'],
    musicSuggestions: ['Nhạc Latin', 'Reggae', 'Pop nhiệt đới']
  },
  // Sản phẩm mới từ các hình ảnh bổ sung
  {
    id: '12',
    name: 'Nến Thơm Big Size Ashwood + Patchouli',
    image: 'uploaded:IMG_4893.PNG-7c94dedf-6c82-477d-a8ea-c0e0ba1637bb',
    description: 'Hương gỗ tần bì (Ashwood) và hoắc hương (Patchouli) mang đến mùi hương mạnh mẽ, đất và ấm áp, lý tưởng cho không gian sang trọng và cá tính.',
    story: 'Được lấy cảm hứng từ những khu rừng cổ thụ đầy rêu phong và bí ẩn, mùi hương này là sự kết hợp hoàn hảo của sự mộc mạc và sang trọng.',
    emotions: ['mạnh mẽ', 'cá tính', 'bí ẩn', 'ấm áp', 'sang trọng'],
    style: ['sang trọng', 'mộc mạc', 'cổ điển'],
    price: '568.000 ₫',
    originalPrice: '1.105.000 ₫',
    weight: '640g',
    burnTime: '90H+',
    scentNotes: ['Gỗ tần bì', 'Hoắc hương'],
    scentProfiles: ['woody', 'earthy', 'warm'],
    musicSuggestions: ['Jazz cổ điển', 'Blues', 'Lo-fi acoustic', 'Ambient forest sounds']
  },
  {
    id: '13',
    name: 'Nến Thơm Serenity - Bạc Hà & Tuyết Tùng (Mint + Cedar)',
    image: 'uploaded:IMG_4894.PNG-5ca3680b-702b-4e6f-a181-64ec8ca493dd',
    description: 'Hương bạc hà tươi mát kết hợp với gỗ tuyết tùng nồng ấm, tạo nên một không gian trong lành, sảng khoái và tập trung.',
    story: 'Mùi hương của sự tỉnh táo và thanh lọc, đưa bạn đến một khu rừng tuyết tùng mát lạnh, nơi tâm trí được gột rửa và tái tạo năng lượng.',
    emotions: ['tươi mát', 'sảng khoái', 'tập trung', 'thanh lọc'],
    style: ['tối giản', 'hiện đại', 'tự nhiên'],
    price: '281.600 ₫',
    originalPrice: '320.000 ₫',
    weight: '110g',
    burnTime: '~25 giờ',
    scentNotes: ['Bạc hà', 'Tuyết tùng'],
    scentProfiles: ['fresh', 'woody', 'clean', 'invigorating'],
    musicSuggestions: ['Chillwave', 'Jazz Fusion', 'Meditation music', 'Ambient']
  },
  {
    id: '14',
    name: 'Nến Thơm Serenity - Gỗ Lũa (Drift Wood)',
    image: 'uploaded:IMG_4895.PNG-0821f114-0593-4203-81f9-4197d1267492',
    description: 'Hương gỗ lũa mộc mạc, mang đậm mùi vị của biển cả và sự ấm áp của gỗ được nắng sấy khô. Tạo không gian bình yên và tự nhiên.',
    story: 'Tưởng tượng mình đang dạo bước trên bãi biển hoang sơ, với những khúc gỗ lũa được sóng biển mài giũa, mang theo câu chuyện của thời gian và tự nhiên.',
    emotions: ['mộc mạc', 'bình yên', 'tự nhiên', 'ấm áp', 'hoài niệm'],
    style: ['vintage', 'tự nhiên', 'ấm cúng'],
    price: '281.600 ₫',
    originalPrice: '320.000 ₫',
    weight: '110g',
    burnTime: '~25 giờ',
    scentNotes: ['Gỗ lũa'],
    scentProfiles: ['woody', 'earthy', 'natural', 'warm'],
    musicSuggestions: ['Acoustic Folk', 'Ambient', 'Relaxing Jazz', 'Lo-fi beats']
  },
  {
    id: '15',
    name: 'Nến Thơm Serenity - Đu Đủ & Ổi (Red Papaya + Guava)',
    image: 'uploaded:IMG_4896.PNG-3194b472-1b84-461c-a2a5-9df14292822b',
    description: 'Hương thơm ngọt ngào và tươi mát của đu đủ chín và ổi, mang lại cảm giác sống động, vui tươi của vùng nhiệt đới.',
    story: 'Khám phá hương vị của một khu vườn trái cây nhiệt đới tràn ngập ánh nắng, nơi mọi giác quan được đánh thức bởi sự tươi mới và ngọt ngào.',
    emotions: ['tươi mát', 'sảng khoái', 'năng lượng', 'vui vẻ', 'ngọt ngào'],
    style: ['năng động', 'trẻ trung', 'tươi sáng'],
    price: '281.600 ₫',
    originalPrice: '320.000 ₫',
    weight: '110g',
    burnTime: '~25 giờ',
    scentNotes: ['Đu đủ', 'Ổi'],
    scentProfiles: ['fruity', 'tropical', 'sweet', 'vibrant'],
    musicSuggestions: ['Tropical House', 'Latin Pop', 'Upbeat World Music', 'Pop sôi động']
  }
];

// Các câu hỏi và lựa chọn cho hành trình cảm xúc
const questions = [
  {
    id: 1,
    text: 'Hôm nay bạn muốn cảm thấy thế nào?',
    options: [
      { text: 'Bình yên và thư giãn', tags: ['bình yên', 'thư giãn', 'tĩnh lặng', 'calming'] },
      { text: 'Đầy năng lượng và tập trung', tags: ['năng lượng', 'tập trung', 'sảng khoái', 'energizing'] },
      { text: 'Lãng mạn và ấm áp', tags: ['lãng mạn', 'tình yêu', 'ấm áp', 'romantic'] },
      { text: 'Trong lành và tinh khiết', tags: ['thanh lọc', 'trong lành', 'tinh khiết', 'clean'] },
      { text: 'Mộc mạc và gần gũi thiên nhiên', tags: ['mộc mạc', 'thiên nhiên', 'giản dị', 'natural'] },
      { text: 'Sang trọng và bí ẩn', tags: ['sang trọng', 'bí ẩn', 'elegant'] },
    ],
  },
  {
    id: 2,
    text: 'Bạn yêu thích nhóm mùi hương nào nhất?',
    options: [
      { text: 'Hương gỗ trầm ấm (Gỗ đàn hương, Trầm hương)', tags: ['woody', 'warm', 'amber', 'earthy'] },
      { text: 'Hương hoa tươi mát (Hoa hồng, Mẫu đơn, Oải hương)', tags: ['floral', 'fresh'] },
      { text: 'Hương trái cây nhiệt đới (Xoài, Cam, Quýt)', tags: ['fruity', 'citrus', 'tropical'] },
      { text: 'Hương ngọt ngào, béo ngậy (Vani, Dừa, Cacao)', tags: ['sweet', 'gourmand'] },
      { text: 'Hương tươi mát, sạch sẽ (Trà xanh, Bông gòn)', tags: ['fresh', 'clean'] },
    ],
  },
  {
    id: 3,
    text: 'Không gian lý tưởng của bạn trông như thế nào?',
    options: [
      { text: 'Tối giản, gọn gàng và hiện đại', tags: ['tối giản', 'hiện đại'] },
      { text: 'Ấm cúng, nhiều đồ gỗ và cây xanh', tags: ['ấm cúng', 'tự nhiên', 'mộc mạc'] },
      { text: 'Sang trọng, tinh tế và lãng mạn', tags: ['sang trọng', 'lãng mạn'] },
      { text: 'Tươi sáng, thoáng đãng và trong lành', tags: ['trong lành', 'tươi mát'] },
      { text: 'Cổ điển, hoài niệm và ấm áp', tags: ['cổ điển', 'vintage', 'ấm áp'] },
    ],
  },
];

// Component chính của ứng dụng
const App = () => {
  const [currentStep, setCurrentStep] = useState(0); // 0: intro, 1-N: questions, N+1: results
  const [userSelections, setUserSelections] = useState([]); // Lưu trữ các tags đã chọn
  const [recommendedProducts, setRecommendedProducts] = useState([]);
  const [showIntro, setShowIntro] = useState(true);
  const [currentThemeClass, setCurrentThemeClass] = useState(''); // Thêm state cho theme

  // Hàm xử lý khi người dùng chọn một câu trả lời
  const handleOptionSelect = (tags) => {
    setUserSelections([...userSelections, ...tags]);
    if (currentStep < questions.length) {
      setCurrentStep(currentStep + 1);
    }
  };

  // Hàm xác định theme giao diện dựa trên lựa chọn của người dùng
  const determineVisualTheme = (selections) => {
    const counts = {};
    selections.forEach(tag => {
      counts[tag] = (counts[tag] || 0) + 1;
    });

    // Ưu tiên theme ấm áp nếu có các tag liên quan đến gỗ/ấm áp
    if (counts['woody'] || counts['warm'] || counts['ấm áp'] || counts['mộc mạc'] || counts['cổ điển'] || counts['sang trọng']) {
      return 'theme-warm-woody';
    }
    // Có thể thêm các điều kiện khác cho các theme khác nếu cần
    // Ví dụ: if (counts['floral'] || counts['fresh']) { return 'theme-fresh-floral'; }
    return ''; // Mặc định hoặc không có theme cụ thể
  };

  // Hàm tính toán sản phẩm gợi ý dựa trên lựa chọn của người dùng
  const calculateRecommendations = () => {
    const tagCounts = {};
    userSelections.forEach(tag => {
      tagCounts[tag] = (tagCounts[tag] || 0) + 1;
    });

    const scoredProducts = products.map(product => {
      let score = 0;
      // Cộng điểm dựa trên tags cảm xúc và phong cách
      product.emotions.forEach(emotionTag => {
        if (tagCounts[emotionTag]) {
          score += tagCounts[emotionTag];
        }
      });
      product.style.forEach(styleTag => {
        if (tagCounts[styleTag]) {
          score += tagCounts[styleTag];
        }
      });
      // Cộng điểm dựa trên scentProfiles (ưu tiên hơn)
      product.scentProfiles.forEach(scentProfileTag => {
        if (tagCounts[scentProfileTag]) {
          score += tagCounts[scentProfileTag] * 2;
        }
      });
      return { ...product, score };
    });

    // Sắp xếp sản phẩm theo điểm số giảm dần và lấy 3 sản phẩm hàng đầu
    const sortedProducts = scoredProducts.sort((a, b) => b.score - a.score);
    // Lọc ra các sản phẩm có điểm số lớn hơn 0
    const filteredProducts = sortedProducts.filter(p => p.score > 0);
    setRecommendedProducts(filteredProducts.slice(0, 3)); // Lấy tối đa 3 sản phẩm
  };

  // useEffect để tính toán gợi ý và xác định theme khi tất cả câu hỏi đã được trả lời
  useEffect(() => {
    if (currentStep === questions.length) {
      calculateRecommendations();
      const theme = determineVisualTheme(userSelections);
      setCurrentThemeClass(theme);
    }
  }, [currentStep, userSelections]);

  // Hàm reset hành trình
  const startNewJourney = () => {
    setCurrentStep(0);
    setUserSelections([]);
    setRecommendedProducts([]);
    setShowIntro(true);
    setCurrentThemeClass(''); // Reset theme khi bắt đầu hành trình mới
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-pink-50 to-rose-100 flex items-center justify-center p-4 font-inter">
      <div className="bg-white rounded-xl shadow-2xl p-8 md:p-12 max-w-2xl w-full text-center border border-rose-200">
        {showIntro ? (
          // Màn hình giới thiệu
          <div className="fade-in">
            <h1 className="text-4xl md:text-5xl font-bold text-rose-700 mb-6">
              MissCandle - Hành Trình Cảm Xúc
            </h1>
            <p className="text-lg text-gray-700 mb-8 leading-relaxed">
              Chào mừng bạn đến với hành trình khám phá mùi hương cá nhân cùng MissCandle. Hãy để chúng tôi giúp bạn tìm thấy nến thơm hoàn hảo, phản ánh chính xác tâm trạng và phong cách của bạn.
            </p>
            <button
              onClick={() => setShowIntro(false)}
              className="bg-rose-500 hover:bg-rose-600 text-white font-semibold py-3 px-8 rounded-full shadow-lg transform transition duration-300 hover:scale-105 focus:outline-none focus:ring-2 focus:ring-rose-400 focus:ring-opacity-75"
            >
              Bắt Đầu Hành Trình
            </button>
          </div>
        ) : currentStep < questions.length ? (
          // Hiển thị các câu hỏi
          <div className="fade-in">
            <h2 className="text-2xl md:text-3xl font-semibold text-rose-600 mb-6">
              Câu hỏi {currentStep + 1}/{questions.length}
            </h2>
            <p className="text-xl text-gray-800 mb-8">
              {questions[currentStep].text}
            </p>
            <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
              {questions[currentStep].options.map((option, index) => (
                <button
                  key={index}
                  onClick={() => handleOptionSelect(option.tags)}
                  className="bg-rose-100 hover:bg-rose-200 text-rose-800 font-medium py-3 px-6 rounded-lg shadow-md transform transition duration-200 hover:scale-105 focus:outline-none focus:ring-2 focus:ring-rose-300 focus:ring-opacity-75 text-base md:text-lg"
                >
                  {option.text}
                </button>
              ))}
            </div>
          </div>
        ) : (
          // Hiển thị kết quả gợi ý
          <div className="fade-in">
            <h2 className="text-3xl md:text-4xl font-bold text-rose-700 mb-6">
              Hành Trình Cảm Xúc Của Bạn Đã Hoàn Thành!
            </h2>
            <p className="text-lg text-gray-700 mb-8">
              Dựa trên những lựa chọn của bạn, MissCandle gợi ý những sản phẩm sau đây để hoàn thiện không gian và cảm xúc của bạn:
            </p>

            {recommendedProducts.length > 0 ? (
              <div className="grid grid-cols-1 gap-8">
                {recommendedProducts.map((product) => (
                  <div key={product.id} className={`rounded-xl shadow-lg p-6 flex flex-col md:flex-row items-center text-left transition-colors duration-500
                    ${currentThemeClass === 'theme-warm-woody' ? 'bg-amber-50 border border-amber-200' : 'bg-white border border-rose-200'}`}>
                    <img
                      src={product.image.startsWith('uploaded:') ? `/download/${product.image.substring(9)}` : product.image}
                      alt={product.name}
                      className="w-48 h-48 object-cover rounded-lg mb-4 md:mb-0 md:mr-6 shadow-md"
                      onError={(e) => { e.target.onerror = null; e.target.src = 'https://placehold.co/400x300/F0F0F0/333333?text=MissCandle'; }}
                    />
                    <div>
                      <h3 className="text-2xl font-semibold text-rose-600 mb-2">{product.name}</h3>
                      <p className="text-gray-700 mb-1">{product.description}</p>
                      <p className="text-gray-600 text-sm mb-1">
                        <span className="font-semibold">Hương chính:</span> {product.scentNotes.join(', ')}
                      </p>
                      <p className="text-gray-600 text-sm mb-3">
                        <span className="font-semibold">Giá:</span> <span className="text-rose-500 font-bold">{product.price}</span>{' '}
                        {product.originalPrice && <span className="line-through text-gray-400">{product.originalPrice}</span>}
                      </p>
                      <p className="text-gray-600 italic text-sm mb-3">"{product.story}"</p>

                      {product.musicSuggestions && product.musicSuggestions.length > 0 && (
                        <div className="mt-3">
                          <p className="font-semibold text-rose-600">Gợi ý âm nhạc:</p>
                          <ul className="list-disc list-inside text-gray-700 text-sm">
                            {product.musicSuggestions.map((music, idx) => (
                              <li key={idx}>{music}</li>
                            ))}
                          </ul>
                        </div>
                      )}

                      <button
                        onClick={() => console.log(`Bạn đã chọn xem chi tiết sản phẩm ${product.name}. Trong ứng dụng thực tế, nút này sẽ dẫn đến trang sản phẩm.`)}
                        className="mt-4 bg-rose-500 hover:bg-rose-600 text-white font-medium py-2 px-5 rounded-full text-sm shadow-md transform transition duration-300 hover:scale-105 focus:outline-none focus:ring-2 focus:ring-rose-400 focus:ring-opacity-75"
                      >
                        Xem chi tiết
                      </button>
                    </div>
                  </div>
                ))}
              </div>
            ) : (
              <p className="text-lg text-gray-600 mb-8">
                Rất tiếc, chúng tôi chưa tìm thấy sản phẩm nào phù hợp hoàn hảo với tất cả các lựa chọn của bạn. Hãy thử lại hành trình hoặc khám phá toàn bộ bộ sưu tập của MissCandle!
              </p>
            )}

            <button
              onClick={startNewJourney}
              className="mt-10 bg-gray-200 hover:bg-gray-300 text-gray-800 font-semibold py-3 px-8 rounded-full shadow-lg transform transition duration-300 hover:scale-105 focus:outline-none focus:ring-2 focus:ring-gray-400 focus:ring-opacity-75"
            >
              Bắt Đầu Hành Trình Mới
            </button>
          </div>
        )}
      </div>
    </div>
  );
};

export default App;
